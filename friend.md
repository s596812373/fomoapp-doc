好友状态:
0:已申请,1:已加好友,2:申请被拒绝
消息类型:
RC:ContactNtf

1.申请加好友
方法: im/friend_pre_add
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)user_id vchar(22) 必填[对方用户Id]
同时将推送消息给对方，消息内容包括
content, user1,user2,relation_id

2.接受或拒绝好友申请
方法: im/friend_react
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)relation_id int 必填[上步推送过来的relation_id]
3)status int    必填[1接受好友，2拒绝好友]
同时将推送消息给对方，消息内容包括
content,user1,user2,status

10.获取好友列表
方法: im/friend_list
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
返回(json格式):
1) status=1 成功,data里面为好友列表详情