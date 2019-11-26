一: 注册APP

1. 开放平台注册:
http://39.108.51.7/openfomo/register.html
2. 开放平台登录地址:
http://39.108.51.7/openfomo/login.html
3. 登录之后，可以增加app，同时在主页获取app列表信息，包括client_id,client_secret等等;

二: 用户获取access_token
0.测试网址,下文标记为base_url:
http://39.108.51.7/fomo/


1.获取认证code:
调用方式: APP客户端调用
方法名称:oauth2/authorize
POST 参数:
1) response_type=code
2) client_id=[申请注册的appid]
3) redirect_uri=[接入方跳转地址]
4) state=[随机字符串]
5)  authorized vchar(22) 必填[同意为"yes",取消为其它任意字符串]
6)  session_key vchar(125) 必填[用户保存在安卓的session_key]
返回：
若成功，status=1,data中的code给第三方用于下一步获取access_token

2. 用获取的code获取access_token和refresh_token
调用方式: 接入方客户端调用
方法名称: oauth2/authorize/token
POST 参数:
1)code: [上步获取的code],
2)client_id: [app id],
3)client_secret: [app secret],
4)grant_type: [填authorization_code],
5)redirect_uri=[接入方跳转地址]
PS： access_token有效期1小时，refresh_token大约2周

4.刷新token
方法名称: /oauth2/RefreshToken
POST 参数
1) refresh_token：上步获得的refresh_token,
2) client_id :
3) client_secret :
4) grant_type : refresh_token,
5) scope : userinfo paytoken

3. 将access_token装入请求api的头部
AUTHORIZATION : Bearer [access_token]


三： 调用api

1. 获取用户信息:
    调用方式： 接入方客户端调用
    方法名称: oauth2/resource/get_user_info
    传递参数: 无
    
2. 用户预充值:
    调用方式: 接入方客户端调用
    方法名称: oauth2/resource/pre_pay
    POST 参数:  
1)  direction=[1为用户向接入方充值，2为接入方向用户转账;这里为1]
2)  timestamp=[当前时间戳，10分钟内有效]
3)  sign=[签名计算公式:sha1(client_secrent+timestamp)]
4)  trade_id=[接入方交易ID]
5)  amount=[充值或转账金额]
6)  coin_id=[币种ID]
7)  client_id=[接入方app id]
8)  server_uri=[必填，后端回调网址]
9)  redirect_uri=[前端回调网址，跟注册app时填写的redirect_uri保持一致]
10) state=[随机字符串]
11) scope=paytoken
12) response_type=code

    返回信息:
1） status: 状态码，详情见附录一
2） sucess: true 成功， false 失败
2） transfer_id: FOMOAPP平台的交易ID，只有在支付成功时返回,

3. 确认支付：   
    调用方式: FOMO客户端调用
    方法名称: oauth2/authorize/confirm_pay
POST 参数:
1）  trade_id=[必填，接入方交易ID]
2）  transfer_id 必填[上一步返回的transfer_id]
3)   wallet_pwd  必填[用户输入的支付密码] 
4)  session_key 必填[保存在FOMOAPP客户端的session_key]
PS: 未确认交易3分钟之后会过期
    返回信息:
1) success: true 为支付成功，false为支付失败
2) status: 状态码
3) msg: 状态说明
4) data: 数组。包含transfer_id: FOMOAPP平台的交易ID; trade_id: 第三方平台的交易ID; 

4. 给用户转账:
    调用方式: 接入方服务端调用(不需要将access_token放入auth头部)
    方法名称: oauth2/authorize/pay_user
POST 参数:  
1)  direction=[1为用户向接入方充值，2为接入方向用户转账;这里为2]
2)  timestamp=[当前时间戳，10分钟内有效]
3)  sign=[签名计算公式:sha1(client_secret+timestamp)]
4)  trade_id=[接入方交易ID]
5)  amount=[转账金额]
6)  coin_id=[币种ID]
7)  user_id=[提现者在FOMO平台的user id]
8)  client_id=[接入方app id]
9)  server_uri=[后端回调网址]
10) state=[随机字符串]
11) scope=paytoken
12) response_type=code
13)  wallet_pwd  [支付密码]

    返回信息:
1) success: true 为支付成功，false为支付失败
2) status: 状态码
3) msg: 状态说明
4) data: 数组。包含transfer_id: FOMOAPP平台的交易ID; trade_id: 第三方平台的交易ID; httpcode: 支付成功第一次回调第三方服务端的HTTP请求状态 

5. 获取交易详情:
   调用方式: 接入方客户端调用 
   方法名称: oauth2/resource/get_transfer_detail
POST 参数:
1) transfer_id =  FOMOAPP平台的交易ID
 返回信息：
1) from_user  数组，发送者的用户信息
2) to_user    数组，接收者的用户信息
3) coin_info  数组, 币种信息
4) transfer_log 数组,交易详情信息

四 ： 接入方需提供的支付回调
请求方式: POST
url: 后端回调网址,预充值时提供的server_uri
传入值：
transfer_id     此订单在FOMO平台的交易ID
user_id         支付用户在FOMO平台的用户ID
trade_id        接入方的交易号
coin_id         支付的币种ID
amount          支付金额
cut_amount      支付产生的手续费
direction       1为用户向接入方充值，2为接入方向用户转账
status          1为支付成功，0为支付失败

注意:
1.从用户转到接入方，或者从接入方转到用户都有回调，且使用同一个回调地址
2.回调失败后会有重试
3.返回200HTTP状态码即为回调成功
4.第一次回调为用户确认充值或者接入方发起提现后1秒钟，第二次到第5次回调每隔1分钟一次，第五次到第一百次每10分钟一次。

附录一：错误码
    0   =>  "参数不足",
    1   =>  "成功",
    2   =>  "用户未登录",
    3   =>  "session错误或用户不存在",
    5   =>  "内部错误",
    6   =>  "状态码不存在",
    7   =>  "签名错误",
    8   =>  "签名过期",
    11  =>  "用户名错误",
    12  =>  "密码错误",
    21  =>  "不支持该币种",
    22  =>  "无法分配地址",
    23  =>  "不存在的币",
    24  =>  "余额不足",
    25  =>  "未设置资金密码",
    26  =>  "资金密码错误",
    54  =>  "没有操作权限",
    74  =>  "该用户不存在",
    80  =>  "client_id格式不符",
    81  =>  "client_id已经注册",
    82  =>  "client_id不存在",
    107 =>  "查询不到用户注册信息",
    211 =>  "接入方交易ID trade_id重复",
    212 =>  "交易ID transfer_id 不存在",
    213 =>  "交易过期",

附录二：获取可用币种信息
方法: base_url+wallet/get_active_coins
参数: 无
返回: 币种信息在active_coins
id: 币ID
chain_id: 币所在的链ID
coin_type: 币的类型,默认为0
price: 币的实时价格
name: 币名称
description: 币简介
bc_explorer: 区块链浏览器地址
decimals: 精度
min_amount: 归集门槛
next_start: 充值扫描下一次起始区块
is_active: 是否可用
sort_id: 排序权重

{
    "success": true,
	"status": 1,
	"msg": "成功",
	"data": {
		"active_coins": [{
			"id": "1",
			"chain_id": "3",
			"coin_type": "0",
			"price": "0.1",
			"name": "vnt",
			"description": "",
			"bc_expolorer": "",
			"contract": "",
			"decimals": "8",
			"min_amount": "1",
			"next_start": "0",
			"is_active": "1",
			"sort_id": "0"
		}, {
			"id": "3",
			"chain_id": "3",
			"coin_type": "0",
			"price": "188.93",
			"name": "eth",
			"description": "",
			"bc_expolorer": "",
			"contract": "",
			"decimals": "18",
			"min_amount": "1",
			"next_start": "6739398",
			"is_active": "1",
			"sort_id": "0"
		}, {
			"id": "5",
			"chain_id": "3",
			"coin_type": "0",
			"price": "0.9993",
			"name": "usdt",
			"description": "",
			"bc_expolorer": "",
			"contract": "0x3551206D38A1D76C3616b8cf2A239Bc3893E4119",
			"decimals": "6",
			"min_amount": "1",
			"next_start": "6739398",
			"is_active": "1",
			"sort_id": "0"
		}, {
			"id": "8",
			"chain_id": "1",
			"coin_type": "0",
			"price": "9027.06",
			"name": "btc",
			"description": "bitcoin",
			"bc_expolorer": "bitcoin.org",
			"contract": "",
			"decimals": "18",
			"min_amount": "1",
			"next_start": "0",
			"is_active": "1",
			"sort_id": "0"
		}]
	}
}    






