1.获取im云上的token
方法: im/get_token
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
返回(json格式):
status=2 未登录，status=0 成功
data 为返回的im云token

2.刷新im信息
方法: im/refresh
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)name vchar(255) 选填[im云上的用户名称]
3)portraitUri vchar(255) 选填[im云上的用户头像地址]
返回(json格式)
status=2 未登录，status=0 成功
data为返回详情

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
data 为返回的详细

5.创建群组
方法： im/group_create
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_name vchar(255) [群组名称]
3)memo vchar(255) [群组备注]
返回(json格式):
status=0 成功, status=1,群组名为空或者错误，status=2 未登录，status=3 融云错误，status=4 数据库错误
data 为返回的详细信息

6.加入群组
方法： im/group_join
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id vchar(25) [群组ID]
返回(json格式):
status=0 成功, status=1,群组ID为空或者错误，status=2 未登录，status=3 融云错误，status=4 群组不存在， status=5,用户已经加入该群组
data 为返回的详细信息

7.发送红包消息
方法: im/msg_redpack_publish
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)toUserId vchar(25)	必填[收信息用户ID]
3)coin_id vchar(25)	    必填[红包币ID]
4）amount float         必填[红包金额]
5）content   vchar(225)  选填(红包备注)
返回(json格式):
1) status=0 成功, data 为数组data['transfer_id']为红包交易Id,需存储在本地
2）status=1 接收红包用户Id 为空
3) status=2 未登录
4) status=3 融云服务器错误，详情查看data
5) status=4 币ID或者币金额为空
6) status=5 币不在用户资产列表中
7) status=6 红包金额错误
8) status=7 余额不足

8.领取红包
方法: im/msg_redpack_accept
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)transfer_id vchar(22) 必填[红包交易Id]
返回(json格式):
1) status=0 成功
2) status=1 红包交易Id transfer_id为空
3）status=2 用户未登录
4) status=3 融云服务器错误，详情查看data
5）status=4 没有可领取的红包
6) status=5 没有权限领取红包

9.加好友
方法: im/friend_add
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)user_id vchar(22) 必填[对方用户Id]
返回(json格式):
1) status=0 成功
2) status=1 user_id为空
3）status=2 用户未登录
4) status=3 用户不存在
5）status=4 已经是好友了,重复添加
6) status=5 该用户无法加为好友
7) status=6 不能添加自己为好友

10.获取好友列表
方法: im/friend_list
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
返回(json格式):
1) status=0 成功,data里面为好友列表详情
2) status=1,好友列表为空
3) status=2,用户未登录

