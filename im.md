1.获取im云上的token
方法: im/get_token
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
返回(json格式):
status=2 未登录，status=1 成功
data 为返回的im云token

2.刷新im信息
方法: im/refresh
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)name vchar(255) 选填[im云上的用户名称]
3)portraitUri vchar(255) 选填[im云上的用户头像地址]


3.获取用户im信息
方法: im/user_info
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
返回(json格式):
status=2 未登录，status=0 成功
data 为返回的im云的用户信息
1)user_id vchar(25) [用户ID]
2)name vchar(255) [im云上的用户名称]
3)portraitUri vchar(255) [im云上的用户头像地址]

4.发送单聊消息
方法: im/msg_private_publish
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)toUserId vchar(25)	必填[收信息用户ID]
3）objectName vchar(128k)	必填[消息类别:文本,语音，小视频等，请参考融云文档]
4)content 必填[消息内容]

5.发送红包或者转账消息
[红包消息类型 MSG:RedPacket; 转账消息类型 MSG:Transfer]
方法: im/msg_redpack_publish
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)toUserId vchar(25)	必填[收信息用户ID]
3)coin_id vchar(25)	    必填[红包币ID]
4）amount float         必填[红包金额]
5) wallet_pwd vchar(25) 必填[支付密码]
6) wrap int(1)          选填[1为红包，2为转账，默认为红包]
7）content   vchar(225)  选填(红包备注)
返回(json格式):
若成功, data 为数组data['transfer_id']为红包交易Id,需存储在本地

6.领取红包或者转账
[红包消息类型 MSG:RedPacket; 转账消息类型 MSG:Transfer]
方法: im/msg_redpack_accept
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)transfer_id vchar(22) 必填[红包交易Id]

7. 发送兑换消息
[兑换消息类型 MSG:Exchange]
方法: im/msg_exchange_publish
参数:
1) session_key vchar(55) 必填[存在本地的用户session]
2) from_coin_id int(11) 必填[转出币ID]
3）from_coin_amount float 必填[转出币金额]
4) toUserId int(11) 必填[对方用户id]
5) to_coin_id   int(11) 必填[兑换币id]
6) to_coin_amount   float   必填[兑换币金额]
7)  wallet_pwd  vchar(55)   必填[资金密码]
8)  content     vchar(125)  选填[消息内容]
返回(json格式):
status=1 成功 data中包含了exchange_id等信息，需存入本地

8. 领取兑换消息
[兑换消息类型 MSG:Exchange]
方法: im/msg_exchange_accept
1) session_key vchar(55) 必填[存在本地的用户session]
2) exchange_id int(11)  必填[兑换交易id]
3)  wallet_pwd  vchar(55)   必填[资金密码]
返回(json格式):
status=1 成功 

9. 获取红包或转账详情
方法: im/msg_get_transfer
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)transfer_id vchar(22) 必填[红包或转账交易Id]
返回：
from_user,发送者的详情
to_user,接收者的详情
coin_info,币种详情
transfer_log,交易详情
在transfer_log数组中:
status: 50待收款，100已收款，400收款过期;
wrap: 1 红包消息交易，2 转账消息交易，0 普通转账交易
{"status":1,"msg":"成功","data":{"from_user":{"id":"8","fomo_id":"ghhu_zh_fl","name":"nima","portraitUri":"http:\/\/localhost\/fomo\/\/uploads\/avatar\/8_1571807678.png","email":null,"phone":"987654321","sex":"0","last_time":"1569404712","status":"0","zip":null,"province":null,"city":null,"address":null,"country":null,"group":"0","add_time":"1569404712","update_time":"1570070271","is_deleted":"0","c_type":"0","extra_attr":null,"friend_money":"3","class":"0"},"to_user":{"id":"7","name":"wocao","icon":"http:\/\/localhost\/martin-bk\/\/uploads\/avatar\/7_1570505849.png","extra":""},"coin_info":{"id":"5","chain_id":"3","coin_type":"0","price":"0.9993","name":"usdt","description":"","bc_expolorer":"","contract":"0x3551206D38A1D76C3616b8cf2A239Bc3893E4119","decimals":"6","min_amount":"1","next_start":"6739398","is_active":"1","sort_id":"0"},"transfer_log":{"id":"8","from_user_id":"8","to_user_id":"7","coin_id":"5","amount":"1.2","action":null,"target_id":"0","memo":"\\\"test
redpack\\\"","status":"400","wrap":"0","update_time":"1573551708","created_time":"1571716300"}}}

10. 获取兑换详情
方法: im/msg_get_exchange
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)exchange_id vchar(22) 必填[兑换交易Id]
返回：
from_user,发送者的详情
to_user,接收者的详情
from_coin_info,转出币币种详情
to_coin_info,转入币币种详情
exchange_log,交易详情
在exchange_log数组中:
status: 50待收款，100已收款，400收款过期;
{"status":1,"msg":"成功","data":{"from_user":{"id":"7","fomo_id":"hudf_zh_fl","name":"wocao","portraitUri":"http:\/\/localhost\/martin-bk\/\/uploads\/avatar\/7_1570505849.png","email":"abc@126.com","phone":"123456789","sex":"0","last_time":"1569404712","status":"0","zip":null,"province":null,"city":null,"address":null,"country":null,"group":"0","add_time":"1569404712","update_time":"1571805707","is_deleted":"0","c_type":"0","extra_attr":null,"friend_money":"-1","class":"3"},"to_user":{"id":"8","fomo_id":"ghhu_zh_fl","name":"nima","portraitUri":"http:\/\/localhost\/fomo\/\/uploads\/avatar\/8_1571807678.png","email":null,"phone":"987654321","sex":"0","last_time":"1569404712","status":"0","zip":null,"province":null,"city":null,"address":null,"country":null,"group":"0","add_time":"1569404712","update_time":"1570070271","is_deleted":"0","c_type":"0","extra_attr":null,"friend_money":"3","class":"0"},"from_coin_info":{"id":"5","chain_id":"3","coin_type":"0","price":"0.9993","name":"usdt","description":"","bc_expolorer":"","contract":"0x3551206D38A1D76C3616b8cf2A239Bc3893E4119","decimals":"6","min_amount":"1","next_start":"6739398","is_active":"1","sort_id":"0"},"to_coin_info":{"id":"3","chain_id":"3","coin_type":"0","price":"188.93","name":"eth","description":"","bc_expolorer":"","contract":"","decimals":"18","min_amount":"1","next_start":"6739398","is_active":"1","sort_id":"0"},"exchange_log":{"id":"3","from_user_id":"7","from_coin_id":"5","from_coin_amount":"100","to_user_id":"8","to_coin_id":"3","to_coin_amount":"50","memo":"兑换红包","status":"100","update_time":"1571808343","create_time":"1571808330"}}}