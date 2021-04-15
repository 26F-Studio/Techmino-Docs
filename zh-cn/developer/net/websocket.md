# <b><font size=7 color=#FFF0E0>Techmino websocket 文档</font></b>
# <b><font size=7 color=#40A0FF>/tech/socket/v1/app</font></b>
## <b><font size=6 color=#50E8B0>请求提升</font></b>
#### <b><font size=4 color=#609070>请求：Header Only</font></b>
#### <b><font size=4 color=#609070>成功返回：消息段内Json</font></b>
```json
{
    "type": "Connect",
    "newestCode": 1300,
    "newestName": "foo",
    "lowest": 1226,
    "notice": "Welcome"
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json + HTTP码</font></b>
>Json格式错误
```json
HTTPCode = 1007    //WrongMessageContent
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
HTTPCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
## <b><font size=6 color=#50E8B0>发送消息</font></b>
### <b><font size=5 color=#F0E070>获取版本信息</font></b>
#### <b><font size=4 color=#609070>请求：消息段内Json</font></b>
```json
{
    "action": 0,
    "data": {"newestOnly": true}
}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json</font></b>
```json
{
    "action": 0,
    "type": "Self",
    "newest": {
      "code": 1300,
      "name": "Alpha 0.13.0",
      "content": "维度突破 New Dimension"
    },
    // include these when newestOnly = false
    "least": {  
      "code": 1226,
      "name": "Alpha V0.12.X",
      "content": "圣诞快乐 Merry Christmas"
    },
    "notice": "Foo and bar"
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json 和/或 关闭码</font></b>
>Json格式错误
```json
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
### <b><font size=5 color=#F0E070>获取公告</font></b>
#### <b><font size=4 color=#609070>请求：消息段内Json</font></b>
```json
{"action": 1}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json</font></b>
```json
{
    "action": 1,
    "type": "Self",
    "notice": "Foo and bar"
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json 和/或 关闭码</font></b>
>Json格式错误
```json
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
# <b><font size=7 color=#40A0FF>/tech/socket/v1/user</font></b>
## <b><font size=6 color=#50E8B0>请求提升</font></b>
#### <b><font size=4 color=#609070>请求：请求体内Json</font></b>
>email + password
```json
{
    "email": "foo@bar.com",
    "password": "FooBarBazQux"
}
```
>id + authToken
```json
{
    "uid": 114,
    "authToken": "B1234EC0-64A8-46E7-964A-12D45ACFC68B"
}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json</font></b>
>email + password
```json
{
    "type": "Connect",
    "uid": 114,
    "authToken": "B1234EC0-64A8-46E7-964A-12D45ACFC68B"
}
```
>id + authToken
```json
{"type": "Connect"}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json + 关闭码</font></b>
>Json格式错误
```json
CloseCode = 1007    //WrongMessageContent
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
>请求字段不匹配
```json
CloseCode = 1008    //Violation
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
## <b><font size=6 color=#50E8B0>发送消息</font></b>
### <b><font size=5 color=#F0E070>获取 accessToken</font></b>
#### <b><font size=4 color=#609070>请求：消息段内Json</font></b>
```json
{"action": 0}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json</font></b>
```json
{
    "action": 0,
    "type": "Self",
    "accessToken": "11451419198109220219211379BE702D1F1C704B012241D47AED7ADA7B824FE6"
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json 和/或 关闭码</font></b>
>Json格式错误
```json
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
>其他错误
```json
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### <b><font size=5 color=#F0E070>获取用户信息</font></b>
#### <b><font size=4 color=#609070>请求：消息段内Json</font></b>
```json
{
    "action": 1,
    "data": {
        "uid": 114,  // default to self info if not given
        "hash": "asdqwniasd12e21edqwad" // default to false if not given
    }
}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json</font></b>
>detailed = true
```json
{
    "action": 1,
    "type": "Self",
    "data": {
        "uid": 114,
        "username": "tadokoro_koji",
        "motto": "yarimasune",
        "avatar": "data:image/jpg;base64,/9j/4AAQS4JxJBjiQT/2Q==" // won't give if hash not exists or equal to server
    }
}
```
>detailed = false
```json
{
    "action": 1,
    "type": "Self",
    "uid": 114,
    "username": "tadokoro_koji"
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json 和/或 关闭码</font></b>
>Json格式错误
```json
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
>玩家不存在
```json
{
    "type": "Error",
    "reason": "ID not found: Foo and Bar"
}
```
# <b><font size=7 color=#40A0FF>/tech/socket/v1/chat</font></b>
## <b><font size=6 color=#50E8B0>请求提升</font></b>
#### <b><font size=4 color=#609070>请求：请求体内Json</font></b>
>id + accessToken
```json
{
    "uid": 114,
    "accessToken": "11451419198109220219211379BE702D1F1C704B012241D47AED7ADA7B824FE6"
}
```
>id + authToken
```json
{
    "uid": 114,
    "authToken": "B1234EC0-64A8-46E7-964A-12D45ACFC68B"
}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json</font></b>
>id + accessToken
```json
{"type": "Connect"}
```
>id+ authToken
```json
{
    "type": "Connect",
    "uid": 114,
    "accessToken": "11451419198109220219211379BE702D1F1C704B012241D47AED7ADA7B824FE6"
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json + 关闭码</font></b>
>Json格式错误
```json
CloseCode = 1007    //WrongMessageContent
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
>请求字段不匹配
```json
CloseCode = 1008    //Violation
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
## <b><font size=6 color=#50E8B0>发送消息</font></b>
### <b><font size=5 color=#F0E070>获取房间列表</font></b>
#### <b><font size=4 color=#609070>请求：消息段内Json</font></b>
```json
{"action": 0}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json</font></b>
```json
{
    "type": "Self",
    "action": 0,
    "roomList": [
        {
            "rid": "global",
            "count": 260,
            "capacity": 1024
        },
        {
            "rid": "english",
            "count": 123,
            "capacity": 512
        },
        {
            "rid": "chinese",
            "count": 242,
            "capacity": 512
        },
    ]
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json 和/或 关闭码</font></b>
>Json格式错误
```json
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>其他错误
```json
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### <b><font size=5 color=#F0E070>接入房间</font></b>
#### <b><font size=4 color=#609070>请求：消息段内Json</font></b>
```json
{
    "action": 1,
    "data": {
        "rid": "global"  //room id
    }
}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json</font></b>
>对连接者（私发）
```json
{
    "type": "Self",
    "action": 1,
    "rid": "global",
    "data": {
        "histories": [
            {
              "uid": 114,
              "username": "tadokoro_koji",
              "time": "2021-02-19 23:08:20.542000",
              "message": "yarimasune!"
            },
            {
                "uid": 111,
                "username": "asefssx",
                "time": "2021-02-19 23:11:54.234000",
                "message": "Foo"
            } // message limit is 10
        ]
    }
}
```
>对房间（排除广播）
```json
{
    "type": "Broadcast",
    "action": 1,
    "rid": "global",
    "data": {
        "uid": 114,
        "username": "tadokoro_koji"
    }
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json 和/或 关闭码</font></b>
>Json格式错误
```json
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
>其他错误
```json
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### <b><font size=5 color=#F0E070>离开房间</font></b>
#### <b><font size=4 color=#609070>请求：消息段内Json</font></b>
```json
{
    "action": 2,
    "data": {
        "rid": "global"  //room id
    }
}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json</font></b>
>对连接者（私发）
```json
{
    "type": "Self",
    "action": 2,
    "rid": "global"
}
```
>对房间（排除广播）
```json
{
    "type": "Broadcast",
    "action": 2,
    "rid": "global",
    "data": {
        "uid": 114,
        "username": "tadokoro_koji"
    }
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json 和/或 关闭码</font></b>
>Json格式错误
```json
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
>其他错误
```json
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### <b><font size=5 color=#F0E070>发送广播消息</font></b>
#### <b><font size=4 color=#609070>请求：消息段内Json</font></b>
```json
{
    "action": 3,
    "data": {
        "rid": "global",
        "message": "yarimasune!"
    }
}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json（房间内全局广播）</font></b>
```json
{
    "type": "Broadcast",
    "action": 3,
    "rid": "global",
    "data": {
        "uid": 114,
        "username": "tadokoro_koji",
        "time": "2021-02-19 23:11:54.234000",
        "message": "yarimasune!"
    }
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json 和/或 关闭码</font></b>
>Json格式错误
```json
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
>其他错误
```json
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
# <b><font size=7 color=#40A0FF>/tech/socket/v1/play</font></b>
## <b><font size=6 color=#50E8B0>请求提升</font></b>
#### <b><font size=4 color=#609070>请求：请求体内Json</font></b>
```json
{
    "uid": 114,
    "accessToken": "11451419198109220219211379BE702D1F1C704B012241D47AED7ADA7B824FE6"
}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json</font></b>
```json
{
    "type": "Connect",
    "action": 0
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json和/或 关闭码</font></b>
>Json格式错误
```json
CloseCode = 1007    //WrongMessageContent
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
>请求字段不匹配
```json
CloseCode = 1008    //Violation
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
## <b><font size=6 color=#50E8B0>发送消息</font></b>
### <b><font size=5 color=#F0E070>获取房间列表</font></b>
#### <b><font size=4 color=#609070>请求：消息段内Json</font></b>
```json
{
    "action": 0,
    "data": {
        "message": "Solo",  // Return all types if not given
        "begin": 10,  // Start from Room 0(first one) if not given
        "count": 15  // Return 10 rooms if not given
    }
}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json</font></b>
```json
{
    "type": "Self",
    "action": 0,
    "roomList": [
        {
            "rid": "jaisn2329joasjd-1234ejo",
            "name": "test 1",
            "type": "classic",
            "private": false,
            "start": true,
            "count": 2,
            "capacity": 6
        },
        {
            "rid": "adfoi3j0923jjeo-asdnq1d",
            "name": "test 2",
            "type": "classic",
            "private": false,
            "start": false,
            "count": 1,
            "capacity": 6
        },
        {
            "rid": "sad120ujosadd13-asn1ndl",
            "name": "test 3",
            "type": "solo",
            "private": true,
            "start": false,
            "count": 1,
            "capacity": 2
        }
    ]
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json 和/或 关闭码</font></b>
>Json格式错误
```json
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>其他错误
```json
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### <b><font size=5 color=#F0E070>创建房间（自动接入）</font></b>
#### <b><font size=4 color=#609070>请求：消息段内Json</font></b>
```json
{
    "action": 1,
    "data": {
        "type": "solo", // Default to "classic" if not given
        "name": "test 4",  // Default to "{typeName} room" if not given
        "password": "114514",  // Default to "" if not given
        "config": "asdqwe12eesad21asasd21d45" // Base64 data 
    }
}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json</font></b>
```json
{
    "type": "Self",
    "action": 2,
    "data": {
        "sid": 0,
        "ready": false,
        "histories": [],
        "players": []
    }
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json 和/或 关闭码</font></b>
>Json格式错误
```json
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
>其他错误
```json
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### <b><font size=5 color=#F0E070>接入房间</font></b>
#### <b><font size=4 color=#609070>请求：消息段内Json</font></b>
```json
{
    "action": 2,
    "data": {
        "rid": "jaisn2329joasjd-1234ejo",  //room id
        "password": "114514",  // Default to "" if not given
        "config": "asdqwe12eesad21asasd21d45"  // Base64 data
    }
}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json</font></b>
>对连接者（私发）
```json
{
    "type": "Self",
    "action": 2,
    "data": {
        "sid": 0,
        "ready": false,
        "histories": [
            {
                "uid": 114,
                "username": "tadokoro_koji",
                "time": "2021-02-19 23:08:20.542000",
                "message": "yarimasune!"
            },
            {
                "uid": 111,
                "username": "asefssx",
                "time": "2021-02-19 23:11:54.234000",
                "message": "Foo"
            },
            {
                "uid": 112,
                "username": "asdfwwe",
                "time": "2021-02-19 23:13:02.123000",
                "message": "Bar"
            } // message limit is 10
        ],
        "players": [
            {
                "sid": 0,
                "uid": 114,
                "username": "tadokoro_koji",
                "config": "absu21k3bkjsaddasdqw123",
                "ready": false
            },
            {
                "sid": 1,
                "uid": 111,
                "username": "asefssx",
                "config": "12jed8a9ihdnio12dqw12he",
                "ready": true
            },
            {
                "sid": 2,
                "uid": 112,
                "username": "asdfwwe",
                "config": "dajoirhy9q3hfouicsdnc12",
                "ready": false
            }
        ]
    }
}
```
>对房间（排除广播）
```json
{
    "type": "Broadcast",
    "action": 2,
    "data": {
        "sid": 0,
        "uid": 114,
        "username": "tadokoro_koji",
        "config": "asdqwe12eesad21asasd21d45",
        "ready": false
    }
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json 和/或 关闭码</font></b>
>Json格式错误
```json
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
>其他错误
```json
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### <b><font size=5 color=#F0E070>离开房间</font></b>
#### <b><font size=4 color=#609070>请求：消息段内Json</font></b>
```json
{"action": 3}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json</font></b>
>对连接者（私发）
```json
{
    "type": "Self",
    "action": 3
}
```
>对房间（排除广播）
```json
{
    "type": "Broadcast",
    "action": 3,
    "data": {
        "uid": 114,
        "username": "tadokoro_koji"
    }
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json 和/或 关闭码</font></b>
>Json格式错误
```json
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
>其他错误
```json
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### <b><font size=5 color=#F0E070>发送广播消息</font></b>
#### <b><font size=4 color=#609070>请求：消息段内Json</font></b>
```json
{
    "action": 4,
    "data": {
        "message": "yarimasune!"
    }
}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json（房间内全局广播）</font></b>
```json
{
    "type": "Broadcast",
    "action": 4,
    "data": {
        "uid": 114,
        "username": "tadokoro_koji",
        "time": "2021-02-19 23:11:54.234000",
        "message": "yarimasune!"
    }
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json 和/或 关闭码</font></b>
>Json格式错误
```json
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
>其他错误
```json
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### <b><font size=5 color=#F0E070>改变游戏配置</font></b>
#### <b><font size=4 color=#609070>请求：消息段内Json</font></b>
```json
{
    "action": 5,
    "data": {
        "config": "123u9812yehiohsenhfo9u8p"
    }
}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json（房间内排除广播）</font></b>
```json
{
    "type": "Broadcast",
    "action": 5,
    "data": {
        "uid": 114,
        "config": "123u9812yehiohsenhfo9u8p"
    }
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json 和/或 关闭码</font></b>
>Json格式错误
```json
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
>其他错误
```json
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### <b><font size=5 color=#F0E070>改变准备状态</font></b>
#### <b><font size=4 color=#609070>请求：消息段内Json</font></b>
```json
{
    "action": 6,
    "data": {"ready": true}
}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json（房间内广播）</font></b>
```json
{
    "type": "Broadcast",
    "action": 6,
    "data": {
        "uid": 114,
        "ready": true
    }
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json 和/或 关闭码</font></b>
>Json格式错误
```json
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
>其他错误
```json
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
## <b><font size=6 color=#50E8B0>接收消息</font></b>
### <b><font size=5 color=#F0E070>准备开始游戏</font></b>
#### <b><font size=4 color=#609070>返回：消息段内Json（房间内全局广播）
```json
{
    "type": "Server",
    "action": 7
}
```
### <b><font size=5 color=#F0E070>正式开始游戏</font></b>
#### <b><font size=4 color=#609070>返回：消息段内Json（房间内全局广播）
```json
{
    "type": "Server",
    "action": 8,
    "data": {"rid": "qwe1203j09di-0123dsdjoiqw"}// Stream room id
}
```
### <b><font size=5 color=#F0E070>结算游戏 （未定）</font></b>
#### <b><font size=4 color=#609070>返回：消息段内Json（房间内全局广播）
```json
{
    "type": "Server",
    "action": 9,
    "data": {
        "start": false,
        "result": [
            {
                "uid": 110,
                "place": 1,
                "score": 12134,
                "survivalTime": 104
            },
            {
                "uid": 111,
                "place": 3,
                "score": 8723,
                "survivalTime": 82
            },
            {
                "uid": 112,
                "place": 2,
                "score": 10521,
                "survivalTime": 101
            },
            {
                "uid": 114,
                "place": 4,
                "score": 7092,
                "survivalTime": 75
            }
        ]
    }
}
```
# <b><font size=7 color=#40A0FF>/tech/socket/v1/stream</font></b>
## <b><font size=6 color=#50E8B0>请求提升</font></b>
#### <b><font size=4 color=#609070>请求：请求体内Json</font></b>
```json
{
    "uid": 114,
    "accessToken": "11451419198109220219211379BE702D1F1C704B012241D47AED7ADA7B824FE6",
    "rid": "qwe1203j09di-0123dsdjoiqw" // Stream room id
}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json</font></b>
>对连接者（私发）
```json
{
    "type": "Connect",
    "data": {
        "connected": [
            110,
            111
        ]
    }
}
```
>对房间（排除广播）
```json
{
    "type": "Broadcast",
    "action": 2,
    "data": {
        "uid": 114,
        "watch": false
    }
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json 和/或 关闭码</font></b>
>Json格式错误
```json
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
>其他错误
```json
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
## <b><font size=6 color=#50E8B0>发送消息</font></b>
### <b><font size=5 color=#F0E070>发送死亡消息</font></b>
#### <b><font size=4 color=#609070>请求：消息段内Json</font></b>
```json
{
    "action": 4,
    "data": {
        "score": 12134,
        "survivalTime": 104
    }
}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json（房间内排除广播）</font></b>
```json
{
    "type": "Broadcast",
    "action": 4,
    "data": {
        "uid": 114,
        "score": 12134,
        "survivalTime": 104
    }
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json 和/或 关闭码</font></b>
>Json格式错误
```json
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
>其他错误
```json
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### <b><font size=5 color=#F0E070>发送录像数据</font></b>
#### <b><font size=4 color=#609070>请求：消息段内Json</font></b>
```json
{
    "action": 5,
    "data": {
        "stream": "23810j0djqe0291esj"
    }
}
```
#### <b><font size=4 color=#609070>成功返回：消息段内Json（房间内排除广播）</font></b>
```json
{
    "type": "Broadcast",
    "action": 5,
    "data": {
        "uid": 114,
        "stream": "23810j0djqe0291esj"
    }
}
```
#### <b><font size=4 color=#609070>失败返回：消息段内Json 和/或 关闭码</font></b>
>Json格式错误
```json
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
>服务器内部错误
```json
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
>其他错误
```json
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
## <b><font size=6 color=#50E8B0>接收消息</font></b>
### <b><font size=5 color=#F0E070>开始游戏</font></b>
#### <b><font size=4 color=#609070>返回：消息段内Json（房间内全局广播）
```json
{
    "type": "Server",
    "action": 0,
    "data": {
        "seed": 9234614  // Random generator seed
    }
}
```
### <b><font size=5 color=#F0E070>结束游戏</font></b>
#### <b><font size=4 color=#609070>返回：消息段内Json（房间内全局广播）
```json
{
    "type": "Server",
    "action": 1
}
```
### <b><font size=5 color=#F0E070>玩家断开连接</font></b>
#### <b><font size=4 color=#609070>返回：消息段内Json（房间内全局广播）
```json
{
    "type": "Server",
    "action": 3,
    "data": {"uid": 114}
}
```
