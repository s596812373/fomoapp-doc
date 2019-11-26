好友状态:
0:已申请,1:已加好友,2:申请被拒绝
消息类型:
RC:ContactNtf

1.申请加好友
方法: im/friend_pre_add
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)user_id vchar(22) 必填[对方用户Id]
3)wallet_pwd vchar(22) 选填[若对方设置好友付费则需要填写]
4）content   vchar(125) 选填[附加消息]
同时将推送消息给对方，消息内容包括
content, user1,user2,relation_id

2.接受或拒绝好友申请
方法: im/friend_react
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)relation_id int 必填[上步推送过来的relation_id]
3)status int    必填[1接受好友，2拒绝好友]
4）content   vchar(125) 选填[附加消息]
同时将推送消息给对方，消息内容包括
content,user1,user2,status

3.获取好友列表
方法: im/friend_list
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
返回(json格式):
1) status=1 成功,data里面为好友列表详情
其中:
class为用户等级
blacklist: 0表示不在黑名单，1表示在黑名单
{
    "success": true,
	"status": 1,
	"msg": "成功",
	"data": {
		"friends": [{
			"user_id": "7",
			"friend_nick": "老婆",
			"name": "wocao",
			"portraitUri": "http:\/\/localhost\/martin-bk\/\/uploads\/avatar\/7_1570505849.png",
			"fomo_id": "hudf_zh_fl",
			"email": "abc@126.com",
			"phone": "123456789",
			"zip": null,
			"province": null,
			"city": null,
			"address": null,
			"country": null,
			"add_time": "1569404712",
			"friend_money": "-1",
			"class": "3",
			"blacklist": "0"
		}]
	}
}



4.搜索好友
方法: im/friend_search
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)search_words vchar(55) 必填[搜索关键词,可以通过用户的名字或者fomo_id进行模糊搜索，也可以通过手机号或者邮箱进行精确查找]
3)page  int(11)     选填[当前页码，默认为1]
4）each_page_count int(5)    选填[每页显示数，默认为20]
返回:
成功结果中包含is_friend,0为不是好友，1为是好友，count,搜索记录总数，可用于分页，其余为用户列表信息
{
    "success": true,
	"status": 1,
	"msg": "成功",
	"data": {
		"page": 1,
		"each_page_count": 20,
		"total": 4,
		"customers": [{
			"is_friend": 0,
			"user_id": "17",
			"fomo_id": "hdfsu_zh_fl",
			"name": "进去公司",
			"portraitUri": "",
			"email": "hu@sf.com",
			"phone": "13444444444",
			"sex": "",
			"province": "",
			"city": "",
			"friend_money": "-1",
			"class": 0
		}, {
			"is_friend": 0,
			"user_id": "1",
			"fomo_id": "hu_zh_fl",
			"name": "果菜游戏公司",
			"portraitUri": "",
			"email": "abc@123.com",
			"phone": "13888888888",
			"sex": "",
			"province": "",
			"city": "",
			"friend_money": "-1",
			"class": 0
		}, {
			"is_friend": 0,
			"user_id": "15",
			"fomo_id": "hu_zh_flbbb",
			"name": "溜溜小公司",
			"portraitUri": "",
			"email": "liuliu@123.com",
			"phone": "13666666666",
			"sex": "",
			"province": "",
			"city": "",
			"friend_money": "-1",
			"class": 0
		}, {
			"is_friend": 0,
			"user_id": "16",
			"fomo_id": "hu_zdfdh_fl",
			"name": "明天公司",
			"portraitUri": "",
			"email": "li@sfs.com",
			"phone": "13555555555",
			"sex": "",
			"province": "",
			"city": "",
			"friend_money": "-1",
			"class": 0
		}]
	}
}



5.获取好友详情
方法: im/friend_detail
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)friend_id   int(11)   必填[要获取好友详情的user_id]
返回:
用户信息在friend_info中：
id:用户ID，
fomo_id,
name,
portraitUri,
email,
phone,
sex,
zip:邮编,
province,
city,
address,
country,
friend_money:-1 不允许加为好友，0 可以通过申请加为好友，其它正数表示加为好友的价格
class: 用户等级，代表总资产换算成比特币的数量等级
friend_nick:好友备注
blacklist : 0 为不在黑名单，1为在黑名单
{
    "success": true,
	"status": 1,
	"msg": "成功",
	"data": {
		"friend_info": {
			"id": "7",
			"fomo_id": "hudf_zh_fl",
			"name": "wocao",
			"portraitUri": "http:\/\/localhost\/martin-bk\/\/uploads\/avatar\/7_1570505849.png",
			"email": "abc@126.com",
			"phone": "123456789",
			"sex": "0",
			"last_time": "1569404712",
			"status": "0",
			"zip": null,
			"province": null,
			"city": null,
			"address": null,
			"country": null,
			"group": "0",
			"add_time": "1569404712",
			"update_time": "1571805707",
			"is_deleted": "0",
			"c_type": "0",
			"extra_attr": null,
			"friend_money": "-1",
			"class": "3",
			"blacklist": "0",
			"friend_nick": "老婆"
		}
	}
}


6.删除好友
方法: im/friend_del
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)friend_id   int(11)   必填[要获取好友详情的user_id]


7.设置好友备注
方法: im/friend_set_nick
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)friend_id   int(11)   必填[要获取好友详情的user_id]
3)nick        vchar(55) 必填[好友备注]


8.加入黑名单
方法: im/friend_add_blacklist
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)friend_id   int(11)   必填[要获取好友详情的user_id]

9.移除黑名单
方法: im/friend_rm_blacklist
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)friend_id   int(11)   必填[要获取好友详情的user_id]
