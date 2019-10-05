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
方法： im/join_create
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id vchar(25) [群组ID]
返回(json格式):
status=0 成功, status=1,群组ID为空或者错误，status=2 未登录，status=3 融云错误，status=4 群组不存在， status=5,用户已经加入该群组
data 为返回的详细信息

