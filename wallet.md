1 获取可用币种信息
方法: wallet/get_active_coins
参数: 无
返回: 币种信息在active_coins
id: 币ID
chain_id: 币所在的链ID
coin_type: 币的类型,默认为0
price: 币的实时价格
name: 币名称
description: 币简介
bc_explorer: 区块链浏览器地址
decimals: 精度
min_amount: 归集门槛
next_start: 充值扫描下一次起始区块
is_active: 是否可用
sort_id: 排序权重

{
    "success": true,
	"status": 1,
	"msg": "成功",
	"data": {
		"active_coins": [{
			"id": "1",
			"chain_id": "3",
			"coin_type": "0",
			"price": "0.1",
			"name": "vnt",
			"description": "",
			"bc_expolorer": "",
			"contract": "",
			"decimals": "8",
			"min_amount": "1",
			"next_start": "0",
			"is_active": "1",
			"sort_id": "0"
		}, {
			"id": "3",
			"chain_id": "3",
			"coin_type": "0",
			"price": "188.93",
			"name": "eth",
			"description": "",
			"bc_expolorer": "",
			"contract": "",
			"decimals": "18",
			"min_amount": "1",
			"next_start": "6739398",
			"is_active": "1",
			"sort_id": "0"
		}, {
			"id": "5",
			"chain_id": "3",
			"coin_type": "0",
			"price": "0.9993",
			"name": "usdt",
			"description": "",
			"bc_expolorer": "",
			"contract": "0x3551206D38A1D76C3616b8cf2A239Bc3893E4119",
			"decimals": "6",
			"min_amount": "1",
			"next_start": "6739398",
			"is_active": "1",
			"sort_id": "0"
		}, {
			"id": "8",
			"chain_id": "1",
			"coin_type": "0",
			"price": "9027.06",
			"name": "btc",
			"description": "bitcoin",
			"bc_expolorer": "bitcoin.org",
			"contract": "",
			"decimals": "18",
			"min_amount": "1",
			"next_start": "0",
			"is_active": "1",
			"sort_id": "0"
		}]
	}
}


2. 获取用户资产列表
方法: wallet/get_user_coins
参数:
1) session_key vchar(55) 必填；
返回(json格式)：
status=1 成功，0参数不足,2用户未登录
data 的user_coins 列表，包含:
1)coin_id [币ID]；
2)chain_id [链ID];
3)coin_name [币名称];
4)price	[资产价格];
5)balance	[总余额];
6)frozen	[冻结金额];
7)available	[可用余额];

3. 获取币的充值地址
方法: wallet/pre_deposit
参数:
1) session_key vchar(55) 必填；
2) chain_id int(6)	[链ID]必填；
返回(json格式)
status=1 成功，0参数不足,2用户未登录，status=21不支持该币种，status=22无法分配地址
data 为 返回的充值地址

4. 提交提现申请
方法: wallet/withdraw
参数:
1) session_key vchar(55) 必填；
2) coin_id int(6)	[币ID]必填；
3) amount int       [提现数量]必填;
4) to     vchar(125)  [提现到地址]必填;
返回(json格式)
status=1 成功，0参数不足,2用户未登录，23不存在的币，24余额不足

{4.针对币种的财务记录}

{5.近期充币的记录地址5个}

