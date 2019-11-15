一: 注册APP

1. 开放平台注册:
http://39.108.51.7/openfomo/register.html
2. 开放平台登录地址:
http://39.108.51.7/openfomo/login.html
3. 登录之后，可以增加app，同时在主页获取app列表信息，包括client_id,client_secret等等;

二: 用户获取access_token
0.测试网址,下文标记为base_url:
http://39.108.51.7/fomo/


1.通过如下跳转页面获取code:
base_url+oauth2/authorize
url携带参数:
response_type=code
client_id=[申请注册的appid]
redirect_uri=[接入方跳转地址]
state=[随机字符串]
scope=[权限容器]
其中:
权限容器：不同权限之间用空格隔开，如: userinfo paytoken

用户提交上述页面信息后跳转到接入方提供的redirect_uri并在浏览器参数中带有code

2. 用获取的code获取access_token和refresh_token
方法名称: /oauth2/authorize/token
传递参数(post 方法):
code: [上步获取的code],
client_id: [app id],
client_secret: [app secret],
grant_type: [填authorization_code或者code],
scope:[容器权限],
state:[随机字符串]

4.刷新token
方法名称: /oauth2/RefreshToken
传递参数(post 方法)
refresh_token：上步获得的refresh_token,
client_id :
client_secret :
grant_type : refresh_token,
scope : userinfo

3. 将access_token装入请求api的头部
AUTHORIZATION : Bearer [access_token]


三： 调用api

1. 获取用户信息:
    调用方式： 客户端调用
    方法名称: oauth2/resource/get_user_info
    传递参数: 无
    
2. 用户充值:
    调用方式: 客户端调用
    跳转页面: 
    oauth2/authorize/pay
    浏览器携带的参数:  
    direction=[1为用户向接入方充值，2为接入方向用户转账;这里为1]
    timestamp=[当前时间戳，10分钟内有效]
    sign=[签名计算公式:sha1(client_secrent+timestamp)]
    trade_id=[接入方交易ID]
    amount=[充值或转账金额]
    coin_id=[币种ID]
    user_id=[充值者在FOMO平台的user id]
    access_token=[前面获取的access_token]
    client_id=[接入方app id]
    server_uri=[后端回调网址]
    redirect_uri=[前端回调网址，跟注册app时填写的redirect_uri保持一致]
    state=[随机字符串]
    scope=paytoken
    response_type=code
    充值成功或失败跳转到接入方网址,携带参数:
    status =1,成功，其它值，失败，具体见附录错误码
    trade_id=[接入方交易ID]
    amount=[充值或转账金额]
    coin_id=[币种ID]
    user_id=[充值者在FOMO平台的user id]
    access_token=[前面获取的access_token]
    client_id=[接入方app id]
    redirect_uri=[回调网址]
    state=[随机字符串]

    前端回调返回信息:
    status: 状态码，请根据附录一查询对应说明
    httpcode: 后端回调返回的HTTP状态
    transfer_id: FOMOAPP平台的交易ID，只有在支付成功时返回

 3. 用户提现:
    调用方式: 服务端调用
    跳转页面: 
    oauth2/authorize/pay
    浏览器携带的参数:  
    direction=[1为用户向接入方充值，2为接入方向用户转账;这里为2]
    timestamp=[当前时间戳，10分钟内有效]
    sign=[签名计算公式:sha1(client_secrent+timestamp)]
    trade_id=[接入方交易ID]
    amount=[充值或转账金额]
    coin_id=[币种ID]
    user_id=[充值者在FOMO平台的user id]
    access_token=[前面获取的access_token]
    client_id=[接入方app id]
    server_uri=[后端回调网址]
    redirect_uri=[后端回调网址，跟注册app时填写的redirect_uri保持一致]
    front_uri=[前端回调网址]
    state=[随机字符串]
    scope=paytoken
    response_type=code
    转账成功或失败跳转到接入方网址,携带参数:
    status =1,成功，其它值，失败，具体见附录错误码
    trade_id=[接入方交易ID]
    amount=[充值或转账金额]
    coin_id=[币种ID]
    user_id=[充值者在FOMO平台的user id]
    access_token=[前面获取的access_token]
    client_id=[接入方app id]
    redirect_uri=[回调网址]
    state=[随机字符串]   

    前端回调返回信息:
    status: 状态码，请根据附录一查询对应说明
    httpcode: 后端回调返回的HTTP状态
    transfer_id: FOMOAPP平台的交易ID，只有在支付成功时返回

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

注意:
1.从用户转到接入方，或者从接入方转到用户都有回调，且使用同一个回调地址
2.回调失败后会有重试
3.返回200HTTP状态码即为回调成功，若不成功每10分钟发起一次回调，超过100次则不再发起






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
            13  =>  "手机格式错误",
            14  =>  "邮箱格式错误",
            15  =>  "手机已注册",
            16  =>  "邮箱已注册",
            17  =>  "base64 string格式错误",
            18  =>  "图片格式错误，只支持png和jpg",
            19  =>  "该fomo id已被占用",
            21  =>  "不支持该币种",
            22  =>  "无法分配地址",
            23  =>  "不存在的币",
            24  =>  "余额不足",
            25  =>  "未设置资金密码",
            26  =>  "资金密码错误",
            41  =>  "融云错误",
            51  =>  "群组名格式错误",
            52  =>  "群组不存在",
            53  =>  "重复加入该群",
            54  =>  "没有操作权限",
            55  =>  "price格式错误",
            56  =>  "coin_id格式错误",
            57  =>  "备注长度有误",
            58  =>  "该用户不在此群组",
            59  =>  "该用户已经是此群组管理员",
            61  =>  "红包金额错误",
            62  =>  "没有可领取的红包或转账",
            63  =>  "没有权限领取红包或转账",
            64  =>  "红包或转账过期",
            65  =>  "非法wrap值",
            66  =>  "用户无权获取消息记录",
            71  =>  "重复添加好友",
            72  =>  "该用户无法加为好友",
            73  =>  "不能添加自己为好友",
            74  =>  "该用户不存在",
            75  =>  "好友申请不存在",
            76  =>  "更新好友失败",
            77  =>  "好友状态不存在",
            78  =>  "该用户不允许加好友",
            79  =>  "该用户不是好友",
            80  =>  "client_id格式不符",
            81  =>  "client_id已经注册",
            82  =>  "client_id不存在",
            101 =>  "用户未设置邀请码",
            102 =>  "邀请码错误",
            103 =>  "重复设置邀请码",
            104 =>  "手机号或者邮箱格式错误",
            105 =>  "验证码错误",
            106 =>  "验证码已过期",
            107 =>  "查询不到用户注册信息",
            108 =>  "绑定前需先解邦",
            109 =>  "手机号或者邮箱未绑定，无法解邦",
            201 =>  "临时验证令牌错误",
            151 =>  "user_list用户组格式错误",
            152 =>  "创建群至少要3个以上用户",
            153 =>  "用户资产不满足本群共识",
            201 =>  "接入方交易ID trade_id重复",

