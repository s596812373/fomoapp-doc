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
min_withdraw: 最低提币额
withdraw_cut: 提币手续费
next_start: 充值扫描下一次起始区块
is_active: 是否可用
sort_id: 排序权重

{
    "success": true,
    "status": 1,
    "msg": "成功",
    "data": {
        "active_coins": [
            {
                "id": "3",
                "chain_id": "3",
                "coin_type": "0",
                "price": "159.56",
                "name": "eth",
                "description": "",
                "bc_expolorer": "",
                "contract": "",
                "decimals": "18",
                "min_amount": "0.0000",
                "min_withdraw": "0.0000",
                "withdraw_cut": "0.0000",
                "next_start": "6739472",
                "is_active": "1",
                "sort_id": "0"
            },
            {
                "id": "5",
                "chain_id": "3",
                "coin_type": "0",
                "price": "0.999",
                "name": "usdt",
                "description": "",
                "bc_expolorer": "",
                "contract": "0x3551206D38A1D76C3616b8cf2A239Bc3893E4119",
                "decimals": "6",
                "min_amount": "1.0000",
                "min_withdraw": "10.0000",
                "withdraw_cut": "10.0000",
                "next_start": "6739393",
                "is_active": "1",
                "sort_id": "0"
            },
            {
                "id": "8",
                "chain_id": "1",
                "coin_type": "0",
                "price": "7573.22",
                "name": "btc",
                "description": "bitcoin",
                "bc_expolorer": "bitcoin.org",
                "contract": "",
                "decimals": "18",
                "min_amount": "1.0000",
                "min_withdraw": "0.0000",
                "withdraw_cut": "0.0000",
                "next_start": "0",
                "is_active": "1",
                "sort_id": "0"
            }
        ]
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
4)logo      [币logo]
5)price	[资产价格];
6)balance	[总余额];
7)frozen	[冻结金额];
8)available	[可用余额];
9)available_cny [可用余额折合成人民币]
data 的 total_btc为所有资产折合成BTC的数量
data 的 total_cny为所有资产折合成人民币的数量
{
    "success": true,
    "status": 1,
    "msg": "成功",
    "data": {
        "user_coins": [
            {
                "coin_id": "3",
                "chain_id": "3",
                "coin_name": "eth",
                "logo": "",
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
                "logo": "",
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
                "logo": "",
                "price": 9027.06,
                "balance": 0,
                "frozen": 0,
                "available": 0,
                "available_cny": 0
            }
        ],
        "total_btc": 7.2312942185617,
        "total_cny": 456941.28752027
    }
}

3. 获取币的充值地址
方法: wallet/pre_deposit
参数:
1) session_key vchar(55) 必填；
2) chain_id int(6)	[链ID]必填；
返回(json格式)
status=1 成功，0参数不足,2用户未登录，status=21不支持该币种，status=22无法分配地址
PS： 不用链的充值格式可能不一样，需要前端分别做不同的渲染
ETH类型地址返回：
{
    "success": true,
    "status": 1,
    "msg": "成功",
    "data": {
        "address": "0x0f4b6555ce73b3172b1bd2ec93a999e52b2f4089"
    }
}
EOS类型地址返回:
{
    "success": true,
    "status": 1,
    "msg": "成功",
    "data": {
        "address": "12345h12345h",
        "memo": "1575016801"
    }
}


4.提现申请第一步
方法: wallet/pre_withdraw
参数:
1) session_key vchar(55) 必填；
2) coin_id int(6)	[币ID]必填；
3) amount decimal(16,4)   [提现数量]必填;
4) to     vchar(125)  [提现到地址]必填;
返回
withdraw_id,下一步发送验证码需要用到

5. 提现第二步，发送验证码
方法: wallet/send_withdraw_verify
参数:
1) session_key vchar(55) 必填；
2）user_name   vchar(125) 必填[发送验证码的手机号或者邮箱];
3)withdraw_id   int     必填[上一步收到的withdraw_id];
返回： 只有状态，成功即进入第三步

6.提现第三步，输入资金密码和验证码并提交
方法: wallet/submit_withdraw
参数:
1) session_key vchar(55) 必填；
2）verify_code   vchar(12) 必填[用户输入的验证码];
3) wallet_pwd   vchar(55)   必填[资金密码];
4) withdraw_id   int     必填[第一步收到的withdraw_id];
返回:
actual_amount 为实际到帐金额
{
    "success": true,
    "status": 1,
    "msg": "成功",
    "data": {
        "actual_amount": "3.0000"
    }
}

7. 获取用户单个币种的所有财务记录
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

8. 获取用户算力以及24小时最低FOMO和TT持币信息
方法: wallet/get_power
1) session_key vchar(55) 必填；
返回:
其中power为算力，min_fomo为用户24小时内所持fomo最小值，min_tt为用户24小时内所持tt最小值
{
    "success": true,
    "status": 1,
    "msg": "成功",
    "data": {
        "min_fomo": 787,
        "min_tt": 200,
        "power": 157
    }
}

9. 获取用户最近的5个充币地址
方法: wallet/history_address
1) session_key vchar(55) 必填；
2) chain_id int(6)	[币的链ID]必填；
返回:
包含最新5个充值地址的数组
{
    "success": true,
    "status": 1,
    "msg": "成功",
    "data": {
        "addresses": [
            "0x343432ldf344324324234324234",
            "0xa85f9db15e8b5562895eba39493acfb39c86c113"
        ]
    }
}

10. 删除用户历史提币地址
方法: wallet/del_address
1) session_key vchar(55) 必填；
2) address vchar(125)	必填[要删除的地址]；
返回:
{
    "success": true,
    "status": 1,
    "msg": "成功",
    "data": null
}
