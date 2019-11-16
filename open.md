一: 注册APP

1. 开放平台注册:
http://39.108.51.7/openfomo/register.html
2. 开放平台登录地址:
http://39.108.51.7/openfomo/login.html
3. 登录之后，可以增加app，同时在主页获取app列表信息，包括client_id,client_secret等等;

二: 用户获取access_token
0.测试网址,下文标记为base_url:
http://39.108.51.7/fomo/


1.通过如下跳转页面获取code(由app客户端调用):
方法名称:oauth2/authorize
GET 参数:
1) response_type=code
2) client_id=[申请注册的appid]
3) redirect_uri=[接入方跳转地址]
4) state=[随机字符串]
POST 参数:
1)  authorized vchar(22) 必填[同意为"yes",取消为其它任意字符串]
2)  session_key vchar(125) 必填[用户保存在安卓的session_key]
用户提交上述页面信息后若成功则跳转到接入方提供的redirect_uri并在浏览器参数中带有code，
如果取消或者失败则返回"error"

2. 用获取的code获取access_token和refresh_token
方法名称: /oauth2/authorize/token
POST 参数:
1)code: [上步获取的code],
2)client_id: [app id],
3)client_secret: [app secret],
4)grant_type: [填authorization_code或者code],
5)scope:[容器权限],
6)state:[随机字符串]

4.刷新token
方法名称: /oauth2/RefreshToken
POST 参数
1) refresh_token：上步获得的refresh_token,
2) client_id :
3) client_secret :
4) grant_type : refresh_token,
5) scope : userinfo

3. 将access_token装入请求api的头部
AUTHORIZATION : Bearer [access_token]


三： 调用api

1. 获取用户信息:
    调用方式： 接入方客户端调用
    方法名称: oauth2/resource/get_user_info
    传递参数: 无
    
2. 用户预充值:
    调用方式: 接入方客户端调用
    方法名称: oauth2/authorize/pre_pay
    GET 参数:  
1) direction=[1为用户向接入方充值，2为接入方向用户转账;这里为1]
2)  timestamp=[当前时间戳，10分钟内有效]
3)  sign=[签名计算公式:sha1(client_secrent+timestamp)]
4)  trade_id=[接入方交易ID]
5)  amount=[充值或转账金额]
6)  coin_id=[币种ID]
7)  user_id=[充值者在FOMO平台的user id]
8)  access_token=[前面获取的access_token]
9)  client_id=[接入方app id]
10)  redirect_uri=[前端回调网址，跟注册app时填写的redirect_uri保持一致]
11) state=[随机字符串]
12) scope=paytoken
13) response_type=code

    前端回调返回信息:
1） status: 状态码，0为失败，1为成功
2） transfer_id: FOMOAPP平台的交易ID，只有在支付成功时返回,下一步通知安卓发起支付时需要用到

 3. 用户提现:
    调用方式: 服务端调用
    方法名称: oauth2/authorize/pre_pay
    GET 参数:  
1) direction=[1为用户向接入方充值，2为接入方向用户转账;这里为2]
2)  timestamp=[当前时间戳，10分钟内有效]
3)  sign=[签名计算公式:sha1(client_secrent+timestamp)]
4)  trade_id=[接入方交易ID]
5)  amount=[充值或转账金额]
6)  coin_id=[币种ID]
7)  user_id=[充值者在FOMO平台的user id]
8)  access_token=[前面获取的access_token]
9)  client_id=[接入方app id]
10)  redirect_uri=[前端回调网址，跟注册app时填写的redirect_uri保持一致]
11) state=[随机字符串]
12) scope=paytoken
13) response_type=code

    前端回调返回信息:
1） status: 状态码，0为失败，1为成功
2） transfer_id: FOMOAPP平台的交易ID，只有在支付成功时返回,下一步通知FOMO前端发起支付时需要用到

4. 确认转账：   
    调用方式: FOMO客户端调用
    方法名称: oauth2/authorize/confirm_pay
    GET 参数:
1)  server_uri=[必填，后端回调网址]
2)  redirect_uri=[必填，前端回调网址，跟注册app时填写的redirect_uri保持一致]
3）  trade_id=[必填，接入方交易ID]
    POST 参数:
1）  transfer_id 必填[上一步返回的transfer_id]
2)  wallet_pwd  必填[用户输入的支付密码] 

四 ： 接入方需提供的支付回调
请求方式: POST
url: 后端回调网址
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
3.返回200HTTP状态码即为回调成功，若不成功每10分钟发起一次回调，超过100次则不再发起

5. 给用户提现:
    调用方式: 服务端调用
    方法名称: oauth2/authorize/payUser
    GET 参数:  
1)  direction=[1为用户向接入方充值，2为接入方向用户转账;这里为2]
2)  timestamp=[当前时间戳，10分钟内有效]
3)  sign=[签名计算公式:sha1(client_secret+timestamp)]
4)  trade_id=[接入方交易ID]
5)  amount=[充值或转账金额]
6)  coin_id=[币种ID]
7)  user_id=[充值者在FOMO平台的user id]
8)  access_token=[前面获取的access_token]
9)  client_id=[接入方app id]
10)  server_uri=[后端回调网址]
11)  redirect_uri=[后端回调网址，跟注册app时填写的redirect_uri保持一致]
12) front_uri=[前端回调网址]
13) state=[随机字符串]
14) scope=paytoken
15) response_type=code
16) 转账成功或失败跳转到接入方网址,携带参数:
17) status =1,成功，其它值，失败，具体见附录错误码
18) trade_id=[接入方交易ID]
19) amount=[充值或转账金额]
20) coin_id=[币种ID]
21) user_id=[充值者在FOMO平台的user id]
22) access_token=[前面获取的access_token]
23) client_id=[接入方app id]
24) redirect_uri=[回调网址]
25) state=[随机字符串]   

    前端回调返回信息:
1)  status: 状态码，请根据附录一查询对应说明
2)  httpcode: 后端回调返回的HTTP状态
3)  transfer_id: FOMOAPP平台的交易ID，只有在支付成功时返回




