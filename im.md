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
返回(json格式)
status=2 未登录，status=0 成功


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
返回(json格式):
status=2 未登录，status=0 成功


5.创建群组
方法： im/group_create
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_name vchar(255) [群组名称]
3)memo vchar(255) [群组备注]
返回(json格式):
status=1 成功, status=41,群组名格式错误，status=41 融云错误


6.加入群组
方法： im/group_join
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id vchar(25) [群组ID]
返回(json格式):
status=43 群组不存在， status=44 用户已经加入该群组

7.发送红包消息
方法: im/msg_redpack_publish
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)toUserId vchar(25)	必填[收信息用户ID]
3)coin_id vchar(25)	    必填[红包币ID]
4）amount float         必填[红包金额]
6) wallet_pwd vchar(25) 必填[支付密码]
5）content   vchar(225)  选填(红包备注)
返回(json格式):
1) status=1 成功, data 为数组data['transfer_id']为红包交易Id,需存储在本地
2) status=41 融云服务器错误，详情查看data
3) status=61 红包金额错误
4) status=24 余额不足
5) status=25 未设置资金密码
6) status=26 资金密码错误

8.领取红包
方法: im/msg_redpack_accept
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)transfer_id vchar(22) 必填[红包交易Id]
返回(json格式):
1）status=62 没有可领取的红包
2) status=63 没有权限领取红包

9.加好友
方法: im/friend_add
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)user_id vchar(22) 必填[对方用户Id]
返回(json格式):
5）status=71 已经是好友了,重复添加
6) status=72 该用户无法加为好友
7) status=73 不能添加自己为好友

10.获取好友列表
方法: im/friend_list
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
返回(json格式):
1) status=1 成功,data里面为好友列表详情

11. 发送兑换消息
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

12. 领取兑换消息
方法: im/msg_exchange_accept
1) session_key vchar(55) 必填[存在本地的用户session]
2) exchange_id int(11)  必填[兑换交易id]
3)  wallet_pwd  vchar(55)   必填[资金密码]
返回(json格式):
status=1 成功 

