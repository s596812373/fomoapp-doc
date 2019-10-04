1. 获取用户资产列表
方法: wallet/get_user_coins
参数:
1) session_key vchar(55) 必填；
返回(json格式)：
status=0 成功，status=2 未登录;
data 为user_coins 列表，包含:
1)coin_id [币ID]；
2)chain_id [链ID];
3)coin_name [币名称];
4)price	[资产价格];
5)balance	[总余额];
6)frozen	[冻结金额];
7)available	[可用余额];

2. 获取币的充值地址
方法: wallet/pre_deposit
参数:
1) session_key vchar(55) 必填；
2) chain_id int(6)	[链ID]必填；
返回(json格式)
status=0 成功，status=2未登录，status=1不支持该币种，status=3无法分配地址
data 为 返回的充值地址


