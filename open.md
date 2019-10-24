一: 注册APP

1. 开放平台注册:
http://39.108.51.7/openfomo/register.html
2. 开放平台登录地址:
http://39.108.51.7/openfomo/login.html
3. 登录之后，可以增加app，同时在主页获取app列表信息，包括client_id,client_secret等等;

二: 用户获取access_token
0.测试网址,后问标记为base_url:
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
权限容器：不同权限之前用空格隔开，如: userinfo paytoken

用户提交上述页面信息后跳转到接入方提供的redirect_uri并在浏览器参数中带有code

2. 用获取的code获取access_token
方法名称: /oauth2/authorize/token
传递参数(post 方法):
code: [上步获取的code],
client_id: [app id],
client_secret: [app secret],
grant_type: [填authorization_code或者code],
scope:[容器权限],
state:[随机字符串]

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
    redirect_uri=[回调网址]
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
    redirect_uri=[回调网址]
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

