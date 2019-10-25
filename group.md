

消息类型:
RC:GrpNtf


1.创建群组
方法： im/group_create
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_name vchar(255) [群组名称]
3)memo vchar(255) [群组备注]
返回(json格式):
status=1 成功, status=41,群组名格式错误，status=41 融云错误

2. 设置加群价格
方法: im/group_set_price
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int 必填[群组ID]
3)price float 必填[价格]
4)coin_id int 必填[币种ID]

3.设置群备注
方法: im/group_set_memo
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int 必填[群组ID]
3)memo vchar(255) [群组备注]

4.获取群组信息
方法: im/group_info
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int 必填[群组ID]

5.加入群组
方法： im/group_join
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id vchar(25) [群组ID]
3)wallet_pwd vchar(25) 选填[若加群价格大于零时则必填]

返回(json格式):
status=43 群组不存在， status=44 用户已经加入该群组