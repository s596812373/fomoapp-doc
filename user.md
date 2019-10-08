1. 手机发送验证码：
方法： customer/sendcode
参数： 
1)phone vchar(20) 必填[手机号]
返回：
json格式，status=0代表正常，其它代表异常，data为结果详情

2. 用户注册:
方法 : customer/insert
参数 : 
1) fomo_id vchar(55) 选填;
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
json格式，status=0正常，1为已注册，2为参数为空，data为结果详情

3. 用户登录：
方法 : customer/login
参数 :
1) phone vchar(20) 必填[手机号]
2) pwd vchar(20) 必填[密码]
返回:
json 格式，status=0 正常，is_login=1已登录，is_login=0未登录，data为用户session_key需存在客户端。

4. 更新用户头像:
方法 : customer/update_avatar
参数 : 
1) session_key vchar(55) 必填[存在本地的用户session]
2) base64_string 必填[图片的base64格式，目前只支持png和jpg两种格式图片]
返回(json格式):
status=0 成功,status=1 base64_string格式错误，status=2 未登录，status=3 图片格式错误
data 若成功，则返回上传头像的url, 否则返回错误详情!

