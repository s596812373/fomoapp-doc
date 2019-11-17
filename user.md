1. 用户注册:
方法: customer/register
参数:
1)user_name vchar(125) 必填[手机号或者邮箱地址]
2)pwd vchar(55) 必填[注册密码,前端验证两次密码是否一致，后端不再验证]
返回:
若成功则返回: verify_id, tmp_user_id等，请保存在本地，下一步需要用到

2. 完成注册:
方法: customer/complete_register
参数:
1)verify_code vchar(8)  必填[邮箱或者手机收到的验证码]
2)verify_id     int(11)     必填[上一步返回的verify_id]
3)tmp_user_id   int(11)     必填[上一步返回的tmp_user_id]
返回:
若成功则返回user_id和fomo_id
{"status":1,"msg":"成功","data":{"user_id":23,"fomo_id":"fm_B0S9D"}}


3. 用户登录：
方法 : customer/login
参数 :
1) login_name vchar(20) 必填[手机号或者邮箱号]
2) pwd vchar(20) 必填[密码]
返回:
session_key, 保存在本地，用于用户将来登录
user_info, 包含用户信息:
id
fomo_id
name
portraitUri: 头像
email:
phone:
sex:
friend_moeny: -1 无法加好友，0,可申请加好友，正数为加好友价格
class: 用户等级
{
    "success": true,
	"status": 1,
	"msg": "成功",
	"data": {
		"session_key": "ad1647c6e14d29b3973ca5b7c0704b5aa1b31730",
		"user_info": {
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
			"update_time": "1573556297",
			"is_deleted": "0",
			"c_type": "0",
			"extra_attr": null,
			"friend_money": "-1",
			"class": "3"
		}
	}
}



4. 更新用户头像:
方法 : customer/update_avatar
参数 : 
1) session_key vchar(55) 必填[存在本地的用户session]
2) base64_string 必填[图片的base64格式，目前只支持png和jpg两种格式图片]
返回(json格式):
status=1 成功,17 base64_string格式错误，18 图片格式错误
data 若成功，则返回上传头像的url, 否则返回错误详情!


5. 获取本人信息:
方法 : customer/get_user_info
参数 : 
1) session_key vchar(55) 必填[存在本地的用户session]
返回:
用户信息在user_info中：
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

6. 设置支付密码:

方法 : customer/set_wallet_pwd
参数 : 
1) session_key vchar(55) 必填[存在本地的用户session]
2) wallet_pwd vchar(55) 必填[钱包支付密码] 
3) tmp_session vchar(125) 必填[第10个接口返回的tmp_session]

7.设置用户信息:

方法 : customer/set_info
参数 : 
1) session_key vchar(55) 必填[存在本地的用户session]
2) name vchar(55) 选填
3) sex  tinyint(0) 选填
4) zip  vchar(20) 选填
5) province vchar(55) 选填
6) city vchar(55) 选填
7) address  vchar(125)  选填
8) country  vchar(55) 选填

8. 设置fomo id:

方法 : customer/set_fomo_id
参数 : 
1) session_key vchar(55) 必填[存在本地的用户session]
2) fomo_id vchar(22) 必填[全网唯一的fomo id号，数字或英文字母组合，不允许中文和空格] 


9. 发送验证码第一步:
方法: customer/rec_pwd1
参数:
1) user_name    vchar(125)  必填[手机号或者邮箱号]
返回(若成功):
verify_id，user_id，这些需要存在本地，在下一步需要用到。

10.发送验证码第二步:
方法: customer/rec_pwd2
参数:
1) verify_id    int(11) 必填[上一步获取的verify_id]
2) user_id      int(11) 必填[上一步获取的user_id]
3) verify_code  vchar(8) 必填[用户输入的验证码]
返回(若成功):
tmp_session, 需要在用户重置密码时用到

11. 重置密码:
方法: customer/set_pwd
参数:
1) tmp_session vchar(125) 必填[第10个接口返回的tmp_session]
2) pwd          vchar(22)  必填[新密码,前端验证两次密码是否一致，后端不再验证]


12. 绑定邮箱第一步:
方法: customer/bind_email
参数:
1) session_key vchar(125) 必填[存在本地的session_code]
2) email    vchar(125)     必填[需要绑定的邮箱地址]
返回:
verify_id: 验证ID，保存在本地，下一步需要

13. 绑定手机第一步:
方法: customer/bind_phone
参数:
1) session_key vchar(125) 必填[存在本地的session_code]
2) phone    vchar(55)     必填[需要绑定的手机号]
返回:
verify_id: 验证ID，保存在本地，下一步需要

14. 完成手机或者邮箱绑定
方法: customer/bind_completed
参数:
1) session_key vchar(125) 必填[存在本地的session_code]
2) verify_code vchar(12)  必填[用户输入的验证码]
3) verfify_id  int(11)    必填[上步收到的存在本地的验证ID]

15. 解邦邮箱第一步:
方法: customer/unbind_email
参数:
1) session_key vchar(125) 必填[存在本地的session_code]
返回:
verify_id: 验证ID，保存在本地，下一步需要

16. 解邦手机第一步:
方法: customer/unbind_phone
参数:
1) session_key vchar(125) 必填[存在本地的session_code]
返回:
verify_id: 验证ID，保存在本地，下一步需要 

17. 完成手机或者邮箱解邦
方法: customer/unbind_completed
参数:
1) session_key vchar(125) 必填[存在本地的session_code]
2) verify_code vchar(12)  必填[用户输入的验证码]
3) verfify_id  int(11)    必填[上步收到的存在本地的验证ID]