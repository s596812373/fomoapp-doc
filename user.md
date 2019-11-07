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
若成功则返回user_id


3. 用户登录：
方法 : customer/login
参数 :
1) login_name vchar(20) 必填[手机号或者邮箱号]
2) pwd vchar(20) 必填[密码]
返回:
json 格式，status=1 正常; status=0 参数不足; status=11 用户名错误; status=12 密码错误;

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
status=1 正常; status=2 未登录; status=3 session错误或者用户不存在

6. 设置支付密码:

方法 : customer/set_wallet_pwd
参数 : 
1) session_key vchar(55) 必填[存在本地的用户session]
2) wallet_pwd vchar(55) 必填[钱包支付密码] 

7.设置用户信息:

方法 : customer/set_info
参数 : 
1) session_key vchar(55) 必填[存在本地的用户session]
2) name vchar(55) 必填
3) sex  vchar(20) 必填
4) zip  vchar(20) 必填
5) province vchar(55) 必填
6) city vchar(55) 必填
7) address  vchar(125)  必填
8) country  vchar(55) 必填

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
session_code, 需要在用户重置密码时用到

11. 重置密码:
方法: customer/set_pwd
参数:
1) session_key vchar(125) 必填[存在本地的session_code]
2) pwd          vchar(22)  必填[新密码,前端验证两次密码是否一致，后端不再验证]
