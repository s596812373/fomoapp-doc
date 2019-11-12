


1. 获取登录用户的每日挖矿数据
方法: customer/get_today_award
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
返回结果：
奖励列表，每项包含:
用户id, award_item(奖励项目),award_type(奖励类型)，amount(奖励金额),coin_id(奖励币种)，target_id(目标ID)


2. 获取登录用户的邀请码信息
方法: customer/get_invitation
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
返回结果：
user_id(用户id), code(用户给他人的邀请码),class(用户等级，0或1), parent_id(用户上级id),parent_code（用户上级邀请码）

3. 设置用户邀请码
方法: customer/set_invitation
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)parent_code   vchar(12) 必填[上级邀请码]
返回结果：
若成功，则返回用户自己的邀请码等信息

{4.获取自己邀请的用户列表}

{5.口令}

{6.预约libra}