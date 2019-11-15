

消息类型:
RC:GrpNtf


1.创建群组
方法： im/group_create
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)user_list vchar(125) 必填[邀请加入群组的用户id列表，用逗号隔开，如："7,8,15"]
3)group_name vchar(255) 选填[群组名称]
4)memo vchar(255) 选填[群组备注]
返回(json格式):
若成功，则返回group_id
{返回所有群组信息}

{设置群图标接口}
{设置群共识}

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
{增加管理员列表}
{增加是否免打扰}
{增加群共识}

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
3)pre_admin int(11) 必填[要设置为管理员的user_id] {一次设多个}

8.删除管理员(只能是群主)
方法:im/group_delete_admin
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]
3)pre_admin int(11) 必填[要设置为管理员的user_id]

9.删除群组成员(只能是管理员)
方法: im/group_delete_user
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]
3)pre_del int(11) 必填[要删除用户的user_id]

10.邀请加群(只能是管理员){一次邀请多个}
方法:im/group_invite_user
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]
3)pre_invite int(11) 必填[要邀请用户的user_id]

11.解散群组(只能是管理员)
方法: im/group_dismiss
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]

12.用戶群组列表
方法: im/user_groups
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
返回:
user_group群组列表信息

13.热门群列表
方法: im/hot_groups
无需参数
返回hot_groups 热门群组信息

14. 主动退群
方法: im/group_quit
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]

15. 搜索群
方法: im/group_search
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)search_words vchar(55) 必填[搜索关键词,可以通过群名或者群备注进行模糊搜索]
3)page  int(11)     选填[当前页码，默认为1]
4）each_page_count int(5)    选填[每页显示数，默认为20]
返回:
成功结果中包含is_join,0为未加入，1为已加入，count,搜索记录总数，可用于分页，其余为群列表信息

16. 设置加群共识(只能是群主)
方法: im/group_set_consensus
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2) group_id int(11) 必填[群组ID]
3)consensus_coin int(11)    必填[共识币ID]
4）consensus_value float     必填[共识币金额]

17. 通过或者拒绝加群申请
方法: im/group_react_join
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]
3)pre_user int(11)  必填[申请者用户ID]
4）action    tinyint(1) 选填[1为通过，2为拒绝，默认为通过]


{
    16.私信，支付，群主收款
}

{
    5. 加入群组时，如果无门槛（无需共识或者支付），需要管理员确认
}