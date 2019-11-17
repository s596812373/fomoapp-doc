

消息类型:
RC:GrpNtf

加群申请，通知管理员消息类型：
MSG:join_group


1.创建群组
方法： im/group_create
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)user_list vchar(125) 必填[邀请加入群组的用户id列表，用逗号隔开，如："7,8,15"]
3)group_name vchar(255) 选填[群组名称]
4)memo vchar(255) 选填[群组备注]
5)logo  选填[base64格式的群图标图片]
返回(json格式):
若成功，则返回group_id,group 信息和logo
{
	"success": true,
	"status": 1,
	"msg": "成功",
	"data": {
		"group_id": 20,
		"group_info": {
			"creator": "8",
			"name": "nima,wocao,物资需,溜溜小公司,明天公司",
			"memo": "",
			"status": 1,
			"update_time": 1573978350,
			"create_time": 1573978350
		}
	}
}


2. 更新群组图标
方法: im/group_update_logo
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int 必填[群组ID]
3)logo  必填[base64格式的群图标图片]
返回:
{
	"success": true,
	"status": 1,
	"msg": "成功",
	"data": {
		"logo_info": "http:\/\/localhost\/fomo\/\/uploads\/group\/19_1573978473.png"
	}
}

3. 设置加群价格
方法: im/group_set_price
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int 必填[群组ID]
3)price float 必填[价格]
4)coin_id int 必填[币种ID]

4.设置群备注
方法: im/group_set_memo
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int 必填[群组ID]
3)memo vchar(255) [群组备注]

5.获取群组信息
方法: im/group_info
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int 必填[群组ID]
返回:
group_info: 群组信息
user_list: 群成员信息
admin_count: 管理员数量
{
	"success": true,
	"status": 1,
	"msg": "成功",
	"data": {
		"group_info": {
			"id": "19",
			"creator": "8",
			"name": "nima,wocao,物资需,溜溜小公司,明天公司",
			"memo": "",
			"price": "0.0000",
			"logo": "http:\/\/localhost\/fomo\/\/uploads\/group\/19_1573978473.png",
			"consensus_coin": "0",
			"consensus_value": "0.0000",
			"private": "0",
			"status": "1",
			"hot": "0",
			"update_time": "1573978473",
			"create_time": "1573978180"
		},
		"user_list": [{
			"id": "47",
			"user_id": "7",
			"group_id": "19",
			"is_admin": "0",
			"is_private": "0",
			"status": "1",
			"fomo_id": "hudf_zh_fl",
			"name": "wocao",
			"portraitUri": "http:\/\/localhost\/martin-bk\/\/uploads\/avatar\/7_1570505849.png",
			"email": "abc@126.com",
			"phone": "123456789",
			"sex": ""
		}, {
			"id": "48",
			"user_id": "9",
			"group_id": "19",
			"is_admin": "0",
			"is_private": "0",
			"status": "1",
			"fomo_id": "wuzixu1123",
			"name": "物资需",
			"portraitUri": "",
			"email": "",
			"phone": "33331111",
			"sex": "1"
		}, {
			"id": "49",
			"user_id": "15",
			"group_id": "19",
			"is_admin": "0",
			"is_private": "0",
			"status": "1",
			"fomo_id": "hu_zh_flbbb",
			"name": "溜溜小公司",
			"portraitUri": "",
			"email": "liuliu@123.com",
			"phone": "13666666666",
			"sex": ""
		}, {
			"id": "50",
			"user_id": "16",
			"group_id": "19",
			"is_admin": "0",
			"is_private": "0",
			"status": "1",
			"fomo_id": "hu_zdfdh_fl",
			"name": "明天公司",
			"portraitUri": "",
			"email": "li@sfs.com",
			"phone": "13555555555",
			"sex": ""
		}, {
			"id": "51",
			"user_id": "8",
			"group_id": "19",
			"is_admin": "1",
			"is_private": "0",
			"status": "1",
			"fomo_id": "ghhu_zh_fl",
			"name": "nima",
			"portraitUri": "http:\/\/localhost\/fomo\/\/uploads\/avatar\/8_1571807678.png",
			"email": "",
			"phone": "987654321",
			"sex": ""
		}],
		"admin_count": 1
	}
}


6.加入群组
方法： im/group_join
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id vchar(25) [群组ID]
3)wallet_pwd vchar(25) 选填[若加群价格大于零时则必填]

7.获取群组成员列表
方法: im/group_user_list
参数:
1) group_id int(11) 必填[群组ID]


8.设置管理员(只能是群主)
方法:im/group_set_admin
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]
3)pre_admin int(11) 必填[要设置为管理员的user_id] {一次设多个}

9.删除管理员(只能是群主)
方法:im/group_delete_admin
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]
3)pre_admin int(11) 必填[要设置为管理员的user_id]

10.删除群组成员(只能是管理员)
方法: im/group_delete_user
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]
3)pre_del int(11) 必填[要删除用户的user_id]

11.邀请加群(只能是管理员){一次邀请多个}
方法:im/group_invite_user
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]
3)pre_invite int(11) 必填[要邀请用户的user_id]

12.解散群组(只能是管理员)
方法: im/group_dismiss
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]

13.用戶群组列表
方法: im/user_groups
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
返回:
user_group群组列表信息

14.热门群列表
方法: im/hot_groups
无需参数
返回hot_groups 热门群组信息

15. 主动退群
方法: im/group_quit
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]

16. 搜索群
方法: im/group_search
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)search_words vchar(55) 必填[搜索关键词,可以通过群名或者群备注进行模糊搜索]
3)page  int(11)     选填[当前页码，默认为1]
4）each_page_count int(5)    选填[每页显示数，默认为20]
返回:
成功结果中包含is_join,0为未加入，1为已加入，count,搜索记录总数，可用于分页，其余为群列表信息

17. 设置加群共识(只能是群主)
方法: im/group_set_consensus
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2) group_id int(11) 必填[群组ID]
3)consensus_coin int(11)    必填[共识币ID]
4）consensus_value float     必填[共识币金额]

18. 通过或者拒绝加群申请
方法: im/group_react_join
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]
3)pre_user int(11)  必填[申请者用户ID]
4）action    tinyint(1) 选填[1为通过，2为拒绝，默认为通过]

19. 设置私聊价格
方法: im/group_set_private
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]
3)price float 必填[价格]

20. 支付私聊
方法: im/group_pay_private
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]
3)wallet_pwd vchar(55) 选填[钱包密码]

21. 查看是否可以在群内私聊对方
方法: im/group_can_private
1)session_key vchar(55) 必填[存在本地的用户session]
2)group_id int(11) 必填[群组ID]
3)to_user int(11) 必填[该群组试图私聊对象ID]
返回:
data中的
can_private:    true 表示可以私聊，false 表示不可以私聊
private_reason: 表示可以或者不可以私聊的原因