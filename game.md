1.新加游戏
方法： game/new
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)client_id   vchar(125) 必填，接入方app id
3)redirect_uri vchar(225) 必填，接入方跳转链接
4)app_name  vchar(55)   选填，游戏名称
5)logo      vchar(255)  选填，游戏logo的链接
6)game_url  vchar(255)  选填，游戏入口链接
7)description   vchar(800)  选填，游戏描述


2.更新游戏
方法： game/update
参数:
1)session_key vchar(55) 必填[存在本地的用户session]
2)client_id   vchar(125) 必填，需要更新的app id，不能更新，只用来查询
3)redirect_uri vchar(225) 选填，接入方跳转链接
4)app_name  vchar(55)   选填，游戏名称
5)logo      vchar(255)  选填，游戏logo的链接
6)game_url  vchar(255)  选填，游戏入口链接
7)description   vchar(800)  选填，游戏描述


3.搜索游戏
方法： game/search
参数:
1) search_words vchar(55) 必填[搜索关键词]
返回:
{
    "success": true,
    "status": 1,
    "msg": "成功",
    "data": {
        "page": 1,
        "each_page_count": 20,
        "total": 3,
        "games": [
            {
                "id": "3",
                "game_app_id": "20",
                "client_id": "yeyeye.io",
                "app_name": "爸爸",
                "logo": "localhost:8080/static/ffff.jpg",
                "game_url": "yeye.io",
                "description": "爷爷的爷爷是谁的爷爷？\n                ",
                "owner_id": "1",
                "recommend": "3",
                "hot": "1",
                "status": "1",
                "update_at": "1574332966",
                "create_at": "1574332917"
            },
            {
                "id": "2",
                "game_app_id": "18",
                "client_id": "gift2.win",
                "app_name": "生化危机",
                "logo": "http://localhost:8080/static/index/images/yzm.png",
                "game_url": "http://gift2.win123",
                "description": "生化危机，世界末日",
                "owner_id": "1",
                "recommend": "1",
                "hot": "2",
                "status": "1",
                "update_at": "1574332797",
                "create_at": "1574332797"
            },
            {
                "id": "1",
                "game_app_id": "19",
                "client_id": "gift2win",
                "app_name": "迎着通吃",
                "logo": "http://47.75.164.117/static/index/images/yzm.png",
                "game_url": "http://gift2.win",
                "description": "迎着通吃，不宜儿科",
                "owner_id": "1",
                "recommend": "2",
                "hot": "3",
                "status": "1",
                "update_at": "1574332434",
                "create_at": "1574332434"
            }
        ]
    }
}

4.游戏列表
方法： game/list
参数:
1) sort_type tinyInt()1 必填[1最新，2推荐，3热门]
返回:
{
    "success": true,
    "status": 1,
    "msg": "成功",
    "data": {
        "page": 1,
        "each_page_count": 20,
        "total": 3,
        "games": [
            {
                "id": "1",
                "game_app_id": "19",
                "client_id": "gift2win",
                "app_name": "迎着通吃",
                "logo": "http://47.75.164.117/static/index/images/yzm.png",
                "game_url": "http://gift2.win",
                "description": "迎着通吃，不宜儿科",
                "owner_id": "1",
                "recommend": "2",
                "hot": "3",
                "status": "1",
                "update_at": "1574332434",
                "create_at": "1574332434"
            },
            {
                "id": "2",
                "game_app_id": "18",
                "client_id": "gift2.win",
                "app_name": "生化危机",
                "logo": "http://localhost:8080/static/index/images/yzm.png",
                "game_url": "http://gift2.win123",
                "description": "生化危机，世界末日",
                "owner_id": "1",
                "recommend": "1",
                "hot": "2",
                "status": "1",
                "update_at": "1574332797",
                "create_at": "1574332797"
            },
            {
                "id": "3",
                "game_app_id": "20",
                "client_id": "yeyeye.io",
                "app_name": "爸爸",
                "logo": "localhost:8080/static/ffff.jpg",
                "game_url": "yeye.io",
                "description": "爷爷的爷爷是谁的爷爷？\n                ",
                "owner_id": "1",
                "recommend": "3",
                "hot": "1",
                "status": "1",
                "update_at": "1574332966",
                "create_at": "1574332917"
            }
        ]
    }
}

