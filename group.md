

消息类型:
RC:GrpNtf


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