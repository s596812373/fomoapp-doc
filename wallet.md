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
8)available_cny [可用余额折合成人民币]
data 的 total_btc为所有资产折合成BTC的数量
data 的 total_cny为所有资产折合成人民币的数量
{
    "success": true,
    "status": 1,
    "msg": "成功",
    "data": {
        "user_coins": [
            {
                "coin_id": "1",
                "chain_id": "3",
                "coin_name": "fomo",
                "price": 0.1,
                "balance": "950.0000",
                "frozen": "0.0000",
                "available": 950,
                "available_cny": 665
            },
            {
                "coin_id": "3",
                "chain_id": "3",
                "coin_name": "eth",
                "price": 188.93,
                "balance": "222.4000",
                "frozen": "0.0000",
                "available": 222.4,
                "available_cny": 294126.224
            },
            {
                "coin_id": "5",
                "chain_id": "3",
                "coin_name": "usdt",
                "price": 0.9993,
                "balance": "23286.5877",
                "frozen": "11.0000",
                "available": 23275.5877,
                "available_cny": 162815.06352027
            },
            {
                "coin_id": "8",
                "chain_id": "1",
                "coin_name": "btc",
                "price": 9027.06,
                "balance": 0,
                "frozen": 0,
                "available": 0,
                "available_cny": 0
            }
        ],
        "total_btc": 7.2418181322169,
        "total_cny": 457606.28752027
    }
}

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

5. 获取用户单个币种的所有财务记录
方法: wallet/coins_record
参数:
1) session_key vchar(55) 必填；
2) coin_id int(6)	[币ID]必填；
返回:
其中记录中的 in_out 为 "in"时表示财产增加，为"out"时表示财产减少；
		   tx_scene 表示财务交易场景,deposit充值，withdraw提现，red_packet且in 为收红包，red_packet且out 为发红包，exchange兑换，reward_*挖矿奖励，星号为奖励场景
{
    "success": true,
    "status": 1,
    "msg": "成功",
    "data": {
        "page": 1,
        "each_page_count": 20,
        "total": 2,
        "records": [
            {
                "id": "1",
                "user_id": "7",
                "coin_id": "1",
                "amount": "100.0000",
                "in_out": "in",
                "tx_scene": "deposit",
                "target_id": "10",
                "create_time": "1574228151",
                "status": 300
            },
            {
                "id": "3",
                "user_id": "7",
                "coin_id": "1",
                "amount": "63.0000",
                "in_out": "in",
                "tx_scene": "deposit",
                "target_id": "9",
                "create_time": "1574228151",
                "status": 300
            }
        ]
    }
}


{5.近期充币的记录地址5个}

