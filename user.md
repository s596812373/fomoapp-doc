1. 手机发送验证码：
方法： customer/sendcode
参数： 
1)phone vchar(20) 必填[手机号]
返回：
json格式，status=1成功,0参数不足，5内部错误

2. 用户注册:
方法 : customer/insert
参数 : 
1) verify_code vchar(55) 必填;
2) name	vchar(255) 选填；
3）username	vchar(55)	选填；
4) portraitUrl vchar(255) 选填 [头像网址]；
5）pwd vchar(20) 必填 [登录密码]；
6) email vchar(55)	选填[登录邮箱]；
7）phone vchar(20)	必填[登录手机号]；
8）sex	 tinyint(1) 选填[性别:0 男，1女];
9) zip vchar(10) 选填[邮编]；
10）province vchar(20) 选填[省份];
11) city vchar(20) 选填[城市]；
12） address vchar(255) 选填[地址];
11） country vchar(20) 选填[国家];
返回: 
json格式，status=1成功，0参数不足，5内部错误, 13电话格式错误，14邮箱格式错误, 15电话已注册，16邮箱已注册
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

