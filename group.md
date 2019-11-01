

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

6.获取群组成员列表
方法: im/group_user_list
参数:
1) group_id int(11) 必填[群组ID]


7.设置管理员(只能是群主)
方法:im/group_set_admin
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]
3)pre_admin int(11) 必填[要设置为管理员的user_id]

8.删除群组成员(只能是管理员)
方法: im/group_delete_user
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]
3)pre_del int(11) 必填[要删除用户的user_id]

9.邀请加群(只能是管理员)
方法:im/group_invite_user
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]
3)pre_invite int(11) 必填[要邀请用户的user_id]