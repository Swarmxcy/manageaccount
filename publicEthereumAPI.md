# API 服务 #
封装区块链服务，用于外部系统与区块链交互。

> **注意:**
> - 所有的URL前缀为/api/v1，如/account/new的完整URL路径是/api/v1/account/new
> - 所有的返回值都包含success，成功为true，失败为false。成功代表提交，不保证写入账本。
> - 当success为false。返回值包含error，内容格式为{code: 100, error_message: "Secret is invalid."}。
> - 在联盟链的每个节点均须部署API；API服务器运行在节点本地，可用localhost访问。


#### 错误代码 ####
```
	SYSTEM  : { code : 101, error_message : 'System Error, please contact support'},
	ADDRESS : { code : 102, error_message : 'Invalid address'},
	SECRET  : { code : 103, error_message : 'Invalid secret'},
	CURRENCY: { code : 104, error_message : 'Invalid currency'},
	GENERAL : { code : 999, error_message : 'Replace the message as needed'}
```

## API 说明 ##

#### 账户 ####

* [生成帐户 - POST - /account/new](#生成帐户)
* [查询帐户（保交所）- POST - /account/allinfo](#查询帐户)
* [查询帐户（保险公司） - POST - /account/myinfo](#查询帐户)
* [添加帐户 - POST - /account/add](#添加帐户)
* [查询帐户数据库 - GET - /account/query](#查询帐户数据库)
* [删除帐户 - POST - /account/del](#删除帐户)
* [更新帐户数据库 - POST - /account/update](#更新帐户数据库)

#### 数据 ####

* [请求数据 - POST - /data/request](#请求数据)
* [请求状态 - GET - /data/request/status/{:month}](#请求状态1)
* [请求状态 - GET - /data/request/status/{:page}/{:size}](#请求状态2)

* [查询数据 - GET - /data/receive/respond/query/{:reqSeq}](#查询数据)
* [数据通知 - GET - /data/receive/respond/notify ](#数据条数通知)
* [请求通知 - GET - /data/receive/request/notify](#请求条数通知)
* [显示请求 - GET - /data/receive/request/query/{:month}](#查询请求1)
* [显示请求 - GET - /data/receive/request/query/{:page}/{:size}](#查询请求2)



#### 超时时间 ####

* [设置超时时间 - POST - /time/change](#设置超时时间)


## 生成帐户 ##

随机生成一个新的帐户，返回地址和私钥。

```
POST /api/v1/account/new
```

*注意:* 有私钥就能完整控制帐户。所以不要将私钥在非安全的服务器和非HTTPS互联网上转输。
#### 输入值 ####
```js
{
  "password":""
}
```

必填参数:

| Field | Type | Description |
|-------|------|-------------|
| password | String | 帐户密码（帐户管理采用web3j的Parity，该密码用户解锁keystore保存的帐户信息） |

#### 返回值 ####

```js
{
    "success": true,
	"account": {
		"address": "0xffDF1F2881f0f8C5b2B572a261c85058D5a113B7",
		"secret": "a549e1e4374b3c69452339a5812c23451072fd67a9a06c37394d0e00f9f70a7b"
	}
}
```
| Field | Type | Description |
|-------|------|-------------|
| address | String | 帐户地址 |
| secret | String | 帐户私钥，传输和存储请注意保密 |

## 查询帐户（保交所） ##

```js
POST /api/v1/account/allinfo
```
#### 输入值 ####
```js
{
  "password":" "
  
}
```

必填参数:

| Field | Type | Description |
|-------|------|-------------|
| password | String | 保交所帐户密码（帐户管理采用web3j的Parity，该密码用户解锁keystore保存的帐户信息） |

#### 返回值 ####

```js
{
  "success": true,
  "accountinformations": [
    {
      "address" : "0x....",
      "companyName" : "火星区块链股份有限公司",
      "respondCount": "50",
      "outRespCount": "10",
      "requestCount": "30"
    },
    {
      "address" : "0x....",
      "companyName" : "木星区块链股份有限公司",
      "respondCount": "60",
      "outRespCount": "30",
      "requestCount": "20"
    }
    ...
  ]
}
```

| Field | Type | Description |
|-------|------|-------------|
| address | String | 帐户地址 |
| companyName | String |公司名称 |
| respondCount | uint | 提供的数据条数 |
| outRespCount | uint | 超时提供的数据条数 |
| requestCount | uint | 请求数据的次数 |

## 查询帐户（保险公司） ##

```
POST /api/v1/account/myinfo
```
#### 输入值 ####
```js
{
  "password":" "
}
```
必填参数:

| Field | Type | Description |
|-------|------|-------------|
| password | String | 保交所帐户密码（帐户管理采用web3j的Parity，该密码用户解锁keystore保存的帐户信息） |

#### 返回值 ####

```js
{
  "success": true,
  "accountinformation": 
    {
      "respondCount": "50",
      "outRespCount": "10",
      "requestCount": "30"
    }
}
```
| Field | Type | Description |
|-------|------|-------------|
| respondCount | uint | 提供的数据条数 |
| outRespCount | uint | 超时提供的数据条数 |
| requestCount | uint | 请求数据的次数 |

## 添加帐户 ##

```js
POST /api/v1/account/add
```
#### 输入值 ####
```js
{
  "password": "", 
  "address":"0x...",
  "comName" : "",
  "comAddr" : "",
  "contact" : "",
  "cellphone":"",
  "weixing":"",
  "owner": ""
  
}
```

输入参数（*为必填）:

| Field | Type | Description |
|-------|------|-------------|
|password（*） | String |  保交所帐户密码（帐户管理采用web3j的Parity，该密码用户解锁keystore保存的帐户信息） |
| address（*） | String | 保险公司账户地址 |
| comName | String | 保险公司名称 |
| comAddr | String | 保险公司地址 |
| contact | String | 保险公司联系人 |
| cellphone | String | 保险公司联系电话 |
| weixing | String | 保险公司联系微信号 |
| owner | String | 保险公司法人代表 |



#### 返回值 ####

```js
{
  "success": true,
  "hash":  "9D591B18EDDD34F0B6CF4223A2940AEA2C3CC778925BABF289E0011CD8FA056E"
}
```

| Field | Type | Description |
|-------|------|-------------|
| hash | String | Transaction Hash |

## 查询帐户数据库 ##

```js
GET /xn/account/query
```
#### 返回值 ####

```js
{
  "success": true,
  "data":  [{"address":"c60f9a08210f5a736a0bee0136e49c324c5d05d9",
             "comName":"太阳3", 
             "comAddr":"太阳路3号", 
             "contact":"段誉", 
             "cellphone":"777777777", 
             "weixing":"段誉",
             "owner":"虚竹"
             },......]
}
```

| Field | Type | Description |
|-------|------|-------------|
| data | 数组 | 每个元素是公司的信息 |
| address | String | 保险公司账户地址 |
| comName | String | 保险公司名称 |
| comAddr | String | 保险公司住址 |
| contact | String | 保险公司联系人 |
| cellphone | String | 保险公司联系电话 |
| weixing | String | 保险公司联系微信号 |
| owner | String | 保险公司法人 |

## 删除帐户 ##

```js
POST /xn/account/del
```
#### 输入值 ####
```js
{
  
  "address":"0x..."
}
```

必填参数:

| Field | Type | Description |
|-------|------|-------------|

| address | String | 保险公司账户地址 |


#### 返回值 ####

```js
{
  "success": true,
  "hash":  "9D591B18EDDD34F0B6CF4223A2940AEA2C3CC778925BABF289E0011CD8FA056E"
}
```

| Field | Type | Description |
|-------|------|-------------|
| hash | String | Transaction Hash |

## 更新帐户数据库 ##

```js
POST /xn/account/update
```
#### 输入值 ####
```js
{
  
  "address":"988a0c462c754cda176763ff0b2f74529744f524",
  "comName":"太阳1",
  "comAddr":"太阳路1号",
  "contact":"乔峰",
  "cellphone":"99999999999",
  "weixing":"乔峰",
  "owner":"虚竹"
}
```

必填参数:

| Field | Type | Description |
|-------|------|-------------|

| address | String | 保险公司账户地址 |
| comName | String | 保险公司名称 |
| comAddr | String | 保险公司住址 |
| contact | String | 保险公司联系人 |
| cellphone | String | 保险公司联系电话 |
| weixing | String | 保险公司联系微信号 |
| owner | String | 保险公司法人 |

#### 返回值 ####

```js
{
  "success": true
}
```

## 请求数据 ##

```js
POST /api/v1/data/request
```
*功能：* 向别的保险公司请求某个被保险人某类险种的保额。

*用法：* 保险公司客户端UI输入被保险人的信息调用该API向智能合约发送数据请求。发送请求一段时间后，可以查询返回的保额数据。

#### 输入值 ####
```js
{
  "password":"",
  "credentialsType":"身份证",
  "number":"21345...",
  "name":"金星",
  "businesstype":"寿险"
}
```

必填参数:

| Field | Type | Description |
|-------|------|-------------|
|password | String |  保险公司帐户密码（帐户管理采用web3j的Parity，该密码用户解锁keystore保存的帐户信息） |
| credentialsType | String | 被保险人的身份证类型 |
| number | String | 被保险人的身份证号 |
| name | String | 被保险人的姓名 |
| businesstype | String | 业务种类 |

#### 返回值 ####

```js
{
  "success": true,
  "hash":  "9D591B18EDDD34F0B6CF4223A2940AEA2C3CC778925BABF289E0011CD8FA056E",
  "reqSeq" : "45"
}
```

| Field | Type | Description |
|-------|------|-------------|
| hash | String | Transaction Hash |
| reqSeq | int  | 节点请求序号。当节点被作为服务器使用时，该序号可以区分一个请求的客户端。客户端需存储该序号，在查询数据时输入。 |

## 请求状态1 ##

```
GET /api/v1/data/request/status/{:month}
```
*功能：* 根据月份保险公司查询自己已发请求的信息及状态。

#### 必填参数 ####

| Field | Type | Description |
|-------|------|-------------|
| month | String | “2” ，第2月份|


#### 返回值 ####

```js
{
  "success": true,
  "size": "50",
  [
  {
  "credentials":  "证件类型",
  "idNumber" : "43522545657567633",
  "name" : "泰森笑死狼",
  "busiType" : "寿险",
  "rstatus" : "响应中",
  "rtime" : "未超时"
  "reqSeq" : "45"
  },
  ...
]
  
}
```

| Field | Type | Description |
|-------|------|-------------|
| rstatus | String | 待响应, 响应中,响应完成 |
| rtime | String | 未超时, 已超时 |
| reqSeq | int  | 节点请求序号。当节点被作为服务器使用时，该序号可以区分一个请求的客户端。客户端需存储该序号，在查询数据时输入。 |


## 请求状态2 ##

```
GET /api/v1/data/request/status/{:page}/{:size}
```
*功能：* 根据要显示的页数和该页的请求数，保险公司查询自己已发请求的信息及状态。

#### 必填参数 ####

| Field | Type | Description |
|-------|------|-------------|
| page | String | “2” ，第二页, 从1开始计数 |
| size | String | “20” ，20个请求 |

#### 返回值 ####

```js

{
  "success": true,
  "size": "50",
  [
  {
  "credentials":  "证件类型",
  "idNumber" : "43522545657567633",
  "name" : "泰森笑死狼",
  "busiType" : "寿险",
  "rstatus" : "响应中",
  "rtime" : "未超时"
  "reqSeq" : "45"
  },
  ...
]
  
}

```

| Field | Type | Description |
|-------|------|-------------|
| rstatus | String | 待响应, 响应中,响应完成 |
| rtime | String | 未超时, 已超时 |
| reqSeq | int  | 节点请求序号。当节点被作为服务器使用时，该序号可以区分一个请求的客户端。客户端需存储该序号，在查询数据时输入。 |


## 查询数据 ##

```
GET /api/v1/data/receive/respond/query/{:reqSeq}
```
*功能：*  根据时间查询被保险人在其它同盟链里保险公司的保额

*用法：*  在保险公司客户端UI，当新数据条数大于0时，操作员可以调用此API根据时间查询所有数据。

#### 输入值 ####

| Field | Type | Description |
|-------|------|-------------|
| reqSeq | String | 请求序号 |


#### 返回值 ####

```js
{
  "success": true,
  "number" : 50,
  "data": [
      {"businessType": "车险",
        "amount":"100",
        "credentialsType":"身份证",
        "number":"22345...",
        "name":"金星",
        "reqSeq" : "45",
        "tout":"标准响应"
      },
      {"businessType": "车险",
        "amount":"200",
        "credentialsType":"身份证",
        "number":"22345...",
        "name":"金星",
        "reqSeq" : "45",
        "tout":"标准响应"
      },
      {"businessType": "车险",
        "amount":"300",
        "credentialsType":"身份证",
        "number":"22345...",
        "name":"金星",
        "reqSeq" : "45",
        "tout":"标准响应"
      }
      ...]
}
```

| Field | Type | Description |
|-------|------|-------------|
| amounts | String | 每份保额 |
| credentialsType | String | 被保险人的证件类型 |
| number | String | 被保险人的证件证号 |
| name | String | 被保险人的姓名 |
| businessType | String | 业务种类 |
| reqSeq | String | 被查询数据的请求序号 |
| tout | String | 标准响应, 超时响应 |


## 数据通知 ##

```
GET /api/v1/data/receive/respond/notify
```
*功能：* 查询API数据库里的有响应新数据已发请求条数

*用法：* 保险公司客户端UI通过短轮询调用该API不断查询是否有新数据，并将获得新数据对应的请求条数显示在UI里。作用是展示智能合约里新数据产生的事件通知并提示操作员可以查询响应数据。
#### 返回值 ####

```js
{
  "success": true,
  "respondCounter": 50
}
```

| Field | Type | Description |
|-------|------|-------------|
| resrespondCounter | int | 新的响应数据条数 |


## 请求条数通知 ##

```
GET /api/v1/data/receive/request/notify
```

*功能：* 查询已经接收到但仍未被显示过请求条数。

*用法：* dapp不断轮询API数据库查询是否有新的请求以便提示操作员有新的请求需要显示。

#### 返回值 ####

```js
{
  "success": true,
  "requestCounter": 20
}

```

| Field | Type | Description |
|-------|------|-------------|
| requestCounter | int | 新请求的条数 |


## 查询请求1 ##

```
GET /api/v1/data/receive/request/query/{:month}
```

*功能：*  根据月份查询所有的请求（响应请求的保险公司即便没有数据也须返回空响应）

#### 输入值 ####

| Field | Type | Description |
|-------|------|-------------|
| month | String | “2” ，第2月份|

#### 返回值 ####

```js
{
  "success": true,
  "number" : 20,
  "requests" : [
	{
    	"hash" : "9D591B18EDDD34F0B6CF4223A2940AEA2C3CC778925BABF289E0011CD8FA056E",
    	"rcvTime":  "2017-2-23",
      "status" : "有数据已返回",      
    	"reqSeq" : "15"
  	}, ... ]
}
```

| Field | Type | Description |
|-------|------|-------------|
| hash | String | 被保险人身份证件和姓名的哈希 |
| rcvTime | String | 接收到请求的时间，格式 yyyy-mm-dd：hh-mm-ss |
| status | String | 该请求的返回状态：未返回，有数据已返回，无数据已返回 |
| reqSeq | int | 请求的序号 |

## 查询请求2 ##

```
GET /api/v1/data/receive/request/query/{:page}/{:size}
```

*功能：*  根据显示页数和请求数查询所有的请求（响应请求的保险公司即便没有数据也须返回空响应）

#### 输入值 ####

| Field | Type | Description |
|-------|------|-------------|
| page | String | “2” ，第二页，从1开始计数|
| size | String | “20” ，20个请求|


#### 返回值 ####

```js
{
  "success": true,
  "number" : 20,
  "requests" : [
	{
    	"hash" : "9D591B18EDDD34F0B6CF4223A2940AEA2C3CC778925BABF289E0011CD8FA056E",
    	"rcvTime":  "2017-2-23",
      "status" : "有数据已返回",      
    	"reqSeq" : "15"
  	}, ... ]
}
```

| Field | Type | Description |
|-------|------|-------------|
| hash | String | 被保险人身份证件和姓名的哈希 |
| rcvTime | String | 接收到请求的时间，格式 yyyy-mm-dd：hh-mm-ss |
| status | String | 该请求的返回状态：未返回，有数据已返回，无数据已返回 |
| reqSeq | int | 请求的序号 |




## 设置超时时间 ##

```
POST /api/v1/time/change
```
#### 输入值 ####
```js
{"password":"",
  "businessType":"寿险",
  "seconds":"50"
}
```
必填参数:

| Field | Type | Description |
|-------|------|-------------|
|password | String |  保险公司帐户密码（帐户管理采用web3j的Parity，该密码用户解锁keystore保存的帐户信息） |
| businesstype | String | 业务种类名称 |
| seconds | int | 超时时间 |

#### 返回值 ####

```js
{
  "success": true,
  "hash":  "9D591B18EDDD34F0B6CF4223A2940AEA2C3CC778925BABF289E0011CD8FA056E"
}
```

| Field | Type | Description |
|-------|------|-------------|
| hash | String | Transaction Hash |


#数据库关系模式#

#### 接收响应 ####
响应数据（id，hash，公钥，业务种类，保额，时间，通知）

*说明:* 请求数据的保险公司节点缓存收到的其它保险公司响应的数据；

*注意:* 主码 id；

*注意:* “公钥”参照“加密公私钥.公钥”。

#### 发送请求 ####
加密公私钥（公钥，私钥，序号，证件类型，身份证号，姓名，业务种类）

*注意:* 发送数据请求时生成的公私钥对及可能需要的方便查询的历史请求的相关身份信息；

*注意:* 主码 公钥。

#### 接收请求 ####
请求数据（hash，公钥，业务种类，响应，时间）

*注意:* 保险公司节点临时性地缓存接收的事件请求，一旦响应了数据，则标记该请求已经返回数据；

*注意:* 主码 公钥。

#### 所有已注册的保险公司帐户（保交所节点用） ####

注册帐户（帐户地址，保险公司名称，联系地址，联系人，联系手机，联系微信号，法人）

*主码：* 帐户地址

属性说明：

| Field | Type | Description |
|-------|------|-------------|
| hash | String | 身份哈希 |
| 身份证件类型 | String | 如护照，身份证 |
| 身份证号 | String | 被保险人身份证号 |
| 姓名 | String | 被保险人姓名 |
| 业务种类 | String | 保险业务种类，如车险、寿险 |
| 保额 | String |  |
| id | bigint | 每条缓存数据的编号 |
| 公钥 | String | 数据加密公钥 |
| 私钥 | String | 数据解密私钥，加密存储 |
| 序号 | int | 该临时公钥对应的请求序号 |
| 时间 | Datetime | 缓存请求或者返回数据的时间 |
| 通知 | boolean | 该条返回数据是否已经通知过客户端：true，通知过；false，仍未通知 |
| 响应 | boolean | 该请求是否已经响应过数据：true，已经响应；false，仍未响应 |
