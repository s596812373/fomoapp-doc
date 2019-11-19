
1.获取登录用户的新手挖矿数据
方法: customer/get_newbee_award
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
返回结果：
奖励列表，每项包含:
用户id, award_item(奖励项目),award_type(奖励类型)，amount(奖励金额),coin_id(奖励币种)，target_id(目标ID,如加群的目标ID就是所加入的群)
{
	"success": true,
	"status": 1,
	"msg": "成功",
	"data": {
		"award_logs": [{
			"id": "22",
			"user_id": "17",
			"award_item": "set_fomo_id",
			"award_type": "newbee",
			"amount": "10.0000",
			"coin_id": "1",
			"target_id": "0",
			"status": "1",
			"create_time": "1573986332",
			"update_time": "1573986332"
		}]
	}
}

2. 获取登录用户的每日挖矿数据
方法: customer/get_today_award
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
返回结果：
奖励列表，每项包含:
用户id, award_item(奖励项目),award_type(奖励类型)，amount(奖励金额),coin_id(奖励币种)，target_id(目标ID,如加群的目标ID就是所加入的群)
{
	"success": true,
	"status": 1,
	"msg": "成功",
	"data": {
		"award_logs": [{
			"id": "19",
			"user_id": "17",
			"award_item": "join_group",
			"award_type": "daily",
			"amount": "10.0000",
			"coin_id": "1",
			"target_id": "19",
			"status": "1",
			"create_time": "1573985312",
			"update_time": "1573985312"
		}, {
			"id": "20",
			"user_id": "17",
			"award_item": "join_group",
			"award_type": "daily",
			"amount": "10.0000",
			"coin_id": "1",
			"target_id": "19",
			"status": "1",
			"create_time": "1573985412",
			"update_time": "1573985412"
		}, {
			"id": "21",
			"user_id": "17",
			"award_item": "join_group",
			"award_type": "daily",
			"amount": "10.0000",
			"coin_id": "1",
			"target_id": "19",
			"status": "1",
			"create_time": "1573985533",
			"update_time": "1573985533"
		}]
	}
}


3. 获取登录用户的邀请码信息
方法: customer/get_invitation
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
返回结果：
user_id(用户id), code(用户给他人的邀请码),class(用户等级，0或1), parent_id(用户上级id),parent_code（用户上级邀请码）

4. 设置用户邀请码
方法: customer/set_invitation
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)parent_code   vchar(12) 必填[上级邀请码]
返回结果：
若成功，则返回用户自己的邀请码等信息,code 为自己的邀请吗
{
	"success": true,
	"status": 1,
	"msg": "成功",
	"data": {
		"user_id": "17",
		"code": "30T5S",
		"class": 1,
		"parent_id": "9",
		"parent_code": "20MFE",
		"update_time": 1573999272,
		"create_time": 1573999272
	}
}

5. 今日签到
方法:	customer/sign_in_today
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
返回结果：
成功:
{
	"success": true,
	"status": 1,
	"msg": "成功",
	"data": {
		"reward_id": 23
	}
}
失败:
{
	"success": false,
	"status": 202,
	"msg": "今日挖矿任务已完成",
	"data": null
}

6.获取自己邀请的用户列表
方法: customer/get_children
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
返回:
children,用户信息列表; count: 总的邀请人数
{
	"success": true,
	"status": 1,
	"msg": "成功",
	"data": {
		"children": [{
			"user_id": "9",
			"user_name": "物资需",
			"portraitUri": null
		}],
		"count": 1
	}
}

7.提交口令
方法: customer/submit_word
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)word		  vchar(12) 必填[口令]
返回:
word_info 口令信息，award_value 用户领取的金额
[PS:需要解决公平分配问题，目前是越在前面抢到的大金额概率越大]
{
    "success": true,
    "status": 1,
    "msg": "成功",
    "data": {
        "word_info": {
            "id": "2",
            "word": "GH78",
            "amount": "134.0000",
            "coin_id": "5",
            "max_count": "5",
            "used_count": "0",
            "used_amount": "0.0000",
            "status": "1",
            "expired": "1574139202",
            "update_time": "1574139202",
            "create_time": "1574139202"
        },
        "award_value": 115.2544
    }
}

8. 查询libra预约人数
方法: customer/subscribe_counts
参数:
无
返回:
num 为libra预约人数
{
    "success": true,
    "status": 1,
    "msg": "成功",
    "data": {
        "num": 3
    }
}

{9.持币分红}