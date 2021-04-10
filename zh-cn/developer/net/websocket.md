# /tech/socket/v1/app

## 请求提升

### 请求形式：Header Only

### 成功返回形式：消息段内 Json

```javascript
{
    "type": "Connect",
    "newestCode": 1300,
    "newestName": "foo",
    "lowest": 1226,
    "notice": "Welcome"
}
```
### 失败返回形式：消息段内 Json + HTTP码

Json 格式错误

```javascript
HTTPCode = 1007    //WrongMessageContent
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
HTTPCode = 1011    //UnexpectedCondition
{
    "type": "Error"
    "reason": "Internal server error"
}
```
## 发送消息

### 获取版本信息

#### 请求形式：消息段内 Json

```javascript
{
    "action":0,
    "data":{
        "newestOnly":true
    }
}
```
#### 成功返回形式：消息段内 Json

newestOnly = true

```javascript
{
    "action": 0,
    "type": "Self",
    "newest":{
      "code": 1300,
      "name": "Alpha 0.13.0",
      "content": "维度突破 New Dimension"
    }
}
```
newestOnly = false
```javascript
{
    "action": 0,
    "type": "Self",
    "newest":{
      "code": 1300,
      "name": "Alpha 0.13.0",
      "content": "维度突破 New Dimension"
    },
    "least":{
      "code": 1226,
      "name": "Alpha V0.12.X",
      "content": "圣诞快乐 Merry Christmas"
    },
    "notice": "Foo and bar"
}
```
#### 失败返回形式：消息段内 Json 和/或 关闭码

Json 格式错误

```javascript
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error"
    "reason": "Internal server error"
}
```
### 获取公告

#### 请求形式：消息段内 Json

```javascript
{
    "action":1
}
```
#### 成功返回形式：消息段内 Json

```javascript
{
    "action": 1,
    "type": "Self",
    "notice": "Foo and bar"
}
```
#### 失败返回形式：消息段内 Json 和/或 关闭码

Json 格式错误

```javascript
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error"
    "reason": "Internal server error"
}
```

# /tech/socket/v1/user

## 请求提升

### 请求形式：请求体内 Json

>email + password
```javascript
{
    "email": "foo@bar.com",
    "password": "FooBarBazQux"
}
```
>id + authToken
```javascript
{
    "uid":114,
    "authToken": "B1234EC0-64A8-46E7-964A-12D45ACFC68B"
}
```
### 成功返回形式：消息段内 Json

>email + password
```javascript
{
    "type": "Connect",
    "uid": 114,
    "authToken": "B1234EC0-64A8-46E7-964A-12D45ACFC68B"
}
```
>id + authToken
```javascript
{
    "type": "Connect"
}
```
### 失败返回形式：消息段内 Json + 关闭码

Json 格式错误

```javascript
CloseCode = 1007    //WrongMessageContent
{
    "type": "Error"
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error"
    "reason": "Internal server error"
}
```
请求字段不匹配
```javascript
CloseCode = 1008    //Violation
{
    "type": "Error"
    "reason": "Foo and bar"
}
```
## 发送消息

### 获取 accessToken

#### 请求形式：消息段内 Json

```javascript
{
    "action":0
}
```
#### 成功返回形式：消息段内 Json

```javascript
{
    "action": 0,
    "type": "Self"，
    "accessToken": "11451419198109220219211379BE702D1F1C704B012241D47AED7ADA7B824FE6"
}
```
#### 失败返回形式：消息段内 Json 和/或 关闭码

Json 格式错误

```javascript
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
其他错误
```lua
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### 获取用户信息

#### 请求形式：消息段内 Json

```javascript
{
    "action": 1，
    "data":{
        "uid": 114,  // return self info if 'id' not exist
        "hash": "asdqwniasd12e21edqwad" // default to false if not given
    }
}
```
#### 成功返回形式：消息段内 Json

>detailed = true
```javascript
{
    "action": 1,
    "type": "Self"，
    "data": {
        "uid": 114,
        "username": "tadokoro_koji",
        "motto": "yarimasune",
        "avatar": "data:image/jpg;base64,/9j/4AAQS4JxJBjiQT/2Q==" // won't give if hash not exists or equal to server
    }
    
}
```
>detailed = false
```javascript
{
    "action": 1,
    "type": "Self"，
    "uid": 114,
    "email": "1145141919@qq.com",
    "username": "tadokoro_koji"
}
```
#### 失败返回形式：消息段内 Json 和/或 关闭码

Json 格式错误

```javascript
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
玩家不存在
```lua
{
    "type": "Error",
    "reason": "ID not found: Foo and Bar"
}
```

# /tech/socket/v1/chat

## 请求提升

### 请求形式：请求体内 Json

>id + accessToken
```javascript
{
    "uid":114,
    "accessToken": "11451419198109220219211379BE702D1F1C704B012241D47AED7ADA7B824FE6"
}
```
>id + authToken
```javascript
{
    "uid":114,
    "authToken": "B1234EC0-64A8-46E7-964A-12D45ACFC68B"
}
```
### 成功返回形式：消息段内 Json

>id + accessToken
```javascript
{
    "type": "Connect"
}
```
>id+ authToken
```javascript
{
    "type": "Connect",
    "uid": 114,
    "accessToken": "11451419198109220219211379BE702D1F1C704B012241D47AED7ADA7B824FE6"
}
```
### 失败返回形式：消息段内 Json + 关闭码

Json 格式错误

```javascript
CloseCode = 1007    //WrongMessageContent
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
请求字段不匹配
```javascript
CloseCode = 1008    //Violation
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
## 发送消息

### 获取房间列表

#### 请求形式：消息段内 Json

```javascript
{
    "action": 0
}
```
#### 成功返回形式：消息段内 Json

```javascript
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
#### 失败返回形式：消息段内 Json 和/或 关闭码

Json 格式错误

```javascript
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
其他错误
```lua
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### 接入房间

#### 请求形式：消息段内 Json

```javascript
{
    "action": 1,
    "data": {
        "rid": "global"  //room id
    }
}
```
#### 成功返回形式：消息段内 Json

>对连接者（私发）
```javascript
{
    "type": "Self",
    "action": 1,
    "rid": "global",
    "data": {
        "histories": [
            {
              "uid": 114,  //user id
              "username": "tadokoro_koji",
              "time": "2021-02-19 23:08:20.542000",
              "message": "yarimasune!"
            },
            {
                "uid": 111,  //user id
                "username": "asefssx",
                "time": "2021-02-19 23:11:54.234000",
                "message": "Foo"
            },
            {
                "uid": 112,  //user id
                "username": "asdfwwe",
                "time": "2021-02-19 23:13:02.123000",
                "message": "Bar"
            } // message limit is 10 
        ]
    }
}
```
>对房间（排除广播）
```javascript
{
    "type": "Broadcast",
    "action": 1,
    "rid": "global",
    "data": {
        "uid": 114,  //user id
        "username": "tadokoro_koji"
    }
}
```
#### 失败返回形式：消息段内 Json 和/或 关闭码

Json 格式错误

```javascript
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
其他错误
```lua
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### 离开房间

#### 请求形式：消息段内 Json

```javascript
{
    "action": 2,
    "data": {
        "rid": "global"  //room id
    }
}
```
#### 成功返回形式：消息段内 Json

>对连接者（私发）
```javascript
{
    "type": "Self",
    "action": 2,
    "rid": "global"
}
```
>对房间（排除广播）
```javascript
{
    "type": "Broadcast",
    "action": 2,
    "rid": "global",
    "data": {
        "uid": 114,  //user id
        "username": "tadokoro_koji"
    }
}
```
#### 失败返回形式：消息段内 Json 和/或 关闭码

Json 格式错误

```javascript
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
其他错误
```lua
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### 发送广播消息

#### 请求形式：消息段内 Json

```javascript
{
    "action": 3,
    "data": {
        "rid": "global",
        "message": "yarimasune!"
    }
}
```
#### 成功返回形式：消息段内 Json（房间内全局广播）

```javascript
{
    "type": "Broadcast",
    "action": 3，
    "rid": "global",
    "data": {
        "uid": 114,  //user id
        "username": "tadokoro_koji",
        "time": "2021-02-19 23:11:54.234000",
        "message": "yarimasune!"
    }
}
```
#### 失败返回形式：消息段内 Json 和/或 关闭码

Json 格式错误

```javascript
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
其他错误
```lua
{
    "type": "Error",
    "reason": "Foo and bar"
}
```

# /tech/socket/v1/play

## 请求提升

### 请求形式：请求体内Json

```javascript
{
    "uid":114,
    "accessToken": "11451419198109220219211379BE702D1F1C704B012241D47AED7ADA7B824FE6"
}
```
### 成功返回形式：消息段内Json

```javascript
{
    "type": "Connect",
    "action": 0
}
```
### 失败返回形式：消息段内 Json和/或 关闭码

Json格式错误

```javascript
CloseCode = 1007    //WrongMessageContent
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
请求字段不匹配
```javascript
CloseCode = 1008    //Violation
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
## 发送消息

### 获取房间列表

#### 请求形式：消息段内 Json

```javascript
{
    "action": 0,
    "data": {
        "message": "Solo",  // Return all types if not given
        "begin": 10,  // Start from Room 0(first one) if not given
        "count": 15  // Return 10 rooms if not given
    }
}
```
#### 成功返回形式：消息段内 Json

```javascript
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
            "capacity": 6,
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
#### 失败返回形式：消息段内 Json 和/或 关闭码

Json格式错误

```javascript
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
其他错误
```lua
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### 创建房间（自动接入）

#### 请求形式：消息段内 Json

```javascript
{
    "action": 1,
    "data": {
        "type": "solo" // Default to "classic" if not given
        "name": "test 4",  // Default to "{typeName} room" if not given
        "password": "114514",  // Default to "" if not given
        "config": "asdqwe12eesad21asasd21d45" // Base64 data 
    }
}
```
#### 成功返回形式：消息段内 Json

```javascript
{
    "type": "Self",
    "action": 2,
    "data": {
        "sid": 0,
        "ready": false
        "histories": [],
        "players": []
    }
}
```
#### 失败返回形式：消息段内 Json 和/或 关闭码

Json 格式错误

```javascript
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
其他错误
```lua
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### 接入房间

#### 请求形式：消息段内 Json

```javascript
{
    "action": 2,
    "data": {
        "rid": "jaisn2329joasjd-1234ejo",  //room id
        "password": "114514",  // Default to "" if not given
        "config": "asdqwe12eesad21asasd21d45"  // Base64 data
    }
}
```
#### 成功返回形式：消息段内 Json

>对连接者（私发）
```javascript
{
    "type": "Self",
    "action": 2,
    "data": {
        "sid": 0,
        "ready": false
        "histories": [
            {
                "uid": 114,  //user id
                "username": "tadokoro_koji",
                "time": "2021-02-19 23:08:20.542000",
                "message": "yarimasune!"
            },
            {
                "uid": 111,  //user id
                "username": "asefssx",
                "time": "2021-02-19 23:11:54.234000",
                "message": "Foo"
            },
            {
                "uid": 112,  //user id
                "username": "asdfwwe",
                "time": "2021-02-19 23:13:02.123000",
                "message": "Bar"
            } // message limit is 10 
        ],
        "players": [
            {
                "sid": 0,
                "uid": 114,  //user id
                "username": "tadokoro_koji",
                "config": "absu21k3bkjsaddasdqw123",
                "ready": false
            },
            {
                "sid": 1,
                "uid": 111,  //user id
                "username": "asefssx",
                "config": "12jed8a9ihdnio12dqw12he",
                "ready": true
            },
            {
                "sid": 2,
                "uid": 112,  //user id
                "username": "asdfwwe",
                "config": "dajoirhy9q3hfouicsdnc12",
                "ready": false
            }
        ]
    }
}
```
>对房间（排除广播）
```javascript
{
    "type": "Broadcast",
    "action": 2,
    "data": {
        "sid": 0,
        "uid": 114,  //user id
        "username": "tadokoro_koji",
        "config": "asdqwe12eesad21asasd21d45",
        "ready": false
    }
}
```
#### 失败返回形式：消息段内 Json 和/或 关闭码

Json 格式错误

```javascript
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
其他错误
```lua
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### 离开房间

#### 请求形式：消息段内 Json

```javascript
{
    "action": 3,
}
```
#### 成功返回形式：消息段内 Json

>对连接者（私发）
```javascript
{
    "type": "Self",
    "action": 3
}
```
>对房间（排除广播）
```javascript
{
    "type": "Broadcast",
    "action": 3,
    "data": {
        "uid": 114,  //user id
        "username": "tadokoro_koji"
    }
}
```
#### 失败返回形式：消息段内 Json 和/或 关闭码

Json 格式错误

```javascript
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
其他错误
```lua
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### 发送广播消息

#### 请求形式：消息段内 Json

```javascript
{
    "action": 4,
    "data": {
        "message": "yarimasune!"
    }
}
```
#### 成功返回形式：消息段内 Json（房间内全局广播）

```javascript
{
    "type": "Broadcast",
    "action": 4，
    "data": {
        "uid": 114,  //user id
        "username": "tadokoro_koji",
        "time": "2021-02-19 23:11:54.234000",
        "message": "yarimasune!"
    }
}
```
#### 失败返回形式：消息段内 Json 和/或 关闭码

Json 格式错误

```javascript
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
其他错误
```lua
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### 改变游戏配置

#### 请求形式：消息段内 Json

```javascript
{
    "action": 5,
    "data": {
        "config": "123u9812yehiohsenhfo9u8p"
    }
}
```
#### 成功返回形式：消息段内 Json（房间内排除广播）

```javascript
{
    "type": "Broadcast",
    "action": 5，
    "data": {
        "uid": 114,  //user id
        "config": "123u9812yehiohsenhfo9u8p"
    }
}
```
#### 失败返回形式：消息段内 Json 和/或 关闭码

Json 格式错误

```javascript
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
其他错误
```lua
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### 改变准备状态

#### 请求形式：消息段内 Json

```javascript
{
    "action": 6,
    "data": {
        "ready": true
    }
}
```
#### 成功返回形式：消息段内 Json（房间内广播）

```javascript
{
    "type": "Broadcast",
    "action": 6,
    "data": {
        "uid": 114,  // user id
        "ready": true
    }
}
```
#### 失败返回形式：消息段内 Json 和/或 关闭码

Json 格式错误

```javascript
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
其他错误
```lua
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
## 接收消息

### 准备开始游戏

#### 返回形式：消息段内 Json（房间内全局广播）

```javascript
{
    "type": "Server",
    "action": 7
}
```
### 正式开始游戏

#### 返回形式：消息段内 Json（房间内全局广播）

```javascript
{
    "type": "Server",
    "action": 8，
    "data": {
        "rid": "qwe1203j09di-0123dsdjoiqw"  // Stream room id
    }
}
```
### 结算游戏 （未定）

#### 返回形式：消息段内 Json（房间内全局广播）

```javascript
{
    "type": "Server",
    "action": 9，
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

# /tech/socket/v1/stream

## 请求提升

### 请求形式：请求体内 Json

```javascript
{
    "uid":114,
    "accessToken": "11451419198109220219211379BE702D1F1C704B012241D47AED7ADA7B824FE6",
    "rid": "qwe1203j09di-0123dsdjoiqw" // Stream room id
}
```
### 成功返回形式：消息段内 Json

>对连接者（私发）
```javascript
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
```javascript
{
    "type": "Broadcast",
    "action": 2,
    "data": {
        "uid": 114,  //user id
        "watch": false
    }
}
```
### 失败返回形式：消息段内 Json 和/或 关闭码

Json 格式错误

```javascript
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
其他错误
```lua
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
## 发送消息

### 发送死亡消息（未确定存活时间由客户端还是服务器计算）

#### 请求形式：消息段内 Json

```javascript
{
    "action": 4,
    "data": {
        "score": 12134,
        "survivalTime": 104
    }
}
```
#### 成功返回形式：消息段内 Json（房间内排除广播）

```javascript
{
    "type": "Broadcast",
    "action": 4，
    "data": {
        "uid": 114,  //user id
        "score": 12134,
        "survivalTime": 104
    }
}
```
#### 失败返回形式：消息段内 Json 和/或 关闭码

Json 格式错误

```javascript
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
其他错误
```lua
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
### 发送录像数据

#### 请求形式：消息段内 Json

```javascript
{
    "action": 5,
    "data": {
        "stream": "23810j0djqe0291esj"
    }
}
```
#### 成功返回形式：消息段内 Json（房间内排除广播）

```javascript
{
    "type": "Broadcast",
    "action": 5，
    "data": {
        "uid": 114,  //user id
        "stream": "23810j0djqe0291esj"
    }
}
```
#### 失败返回形式：消息段内 Json 和/或 关闭码

Json 格式错误

```javascript
{
    "type": "Warn",
    "reason": "Foo and bar"
}
```
服务器内部错误
```javascript
CloseCode = 1011    //UnexpectedCondition
{
    "type": "Error",
    "reason": "Internal server error"
}
```
其他错误
```lua
{
    "type": "Error",
    "reason": "Foo and bar"
}
```
## 接收消息

### 开始游戏

#### 返回形式：消息段内 Json（房间内全局广播）

```javascript
{
    "type": "Server",
    "action": 0，
    "data": {
        "seed": 9234614  // Random generator seed
    }
}
```
### 结束游戏

#### 返回形式：消息段内 Json（房间内全局广播）

```javascript
{
    "type": "Server",
    "action": 1
}
```
### 玩家断开连接

#### 返回形式：消息段内 Json（房间内全局广播）

```javascript
{
    "type": "Server",
    "action": 3
    "data": {
        "uid": 114  //user id
    }
}
```
