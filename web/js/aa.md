#总则
## 前缀
- 原有接口保持前缀不变
- 本次讨论的接口前缀/v2
## Json规范
所有value采用json string类型

## Request请求
### Request Body
- 采用json格式

### 签名
- 智凡提供的接口参考智凡接口签名规则
- 风行提供的接口参考风行接口签名规则

## Response响应

### 状态相应码
#### http状态码
- 参见http协议
#### 业务逻辑响应码
| 响应码 | 响应消息模版        |
|:------:|:-------------------:|
|  000   | 操作成功            |
|  101   | 库存不足            |
|  201   | 订单已出库无法追单  |
|  301   | 商品编号不存在      |

### 通用响应格式
#### 参数列表
| 参数    | 释义           | 类型        | 备注         |
|:-------:|:--------------:|:-----------:|:------------:|
| code    | 业务逻辑响应码 | String      | -            |
| desc    | 响应消息       | String      | -            |
| data    | 返回数据实体   | Json Object | 没有返回数据时，不返回该属性。参见具体接口 |
#### 示例
```json
{
    "code": "000", 
    "desc": "操作成功",
    "data": {...}
}
```

# api设计
## 提交订单确认库存接口（ERP提供，平台调用）
### Request
#### URI
/v2/stocks
#### Method
PUT
#### Body
| 参数       | 释义     |  类型   | 必填 | 备注 |  
|:----------:|:--------:|:-------:|:----:|:----:|
| goodsNum   | 商品编号 | String  | Y    | -    |
| goodsName  | 商品名称 | String  | N    | -    |
| goodsCount | 下单数量 | Integer | Y    | -    |
```json
{
    "goodsList": [
        {
            "goodsNum": "001",
            "goodsName": "商品1",
            "goodsCount": "2"
        },
        {
            "goodsNum": "003",
            "goodsName": "商品3",
            "goodsCount": "1",
        },
        ...
    ]
}
```
### Response
#### 状态响应码000
```json
{
    "code": "000", 
    "desc": "操作成功"
}
```
##### 状态响应码101
| 参数       | 释义     |  类型   |
|:----------:|:--------:|:-------:|
| goodsLeft  | 剩余库存 | Integer |
```json
{
    "code": "101", 
    "desc": "库存不足",
    "data": [
        {
            "goodsNum": "001",
            "goodsName": "商品1",
            "goodsLeft": "1",
        },
        ...
    ]
}
```
## 实时库存查询接口（ERP提供，平台调用）
### Request
#### URI
/v2/stocks/{goodsNum}
#### Method
GET
#### Parameter
| 参数       | 释义     |  类型   | 必填 |
|:----------:|:--------:|:-------:|:----:|
| goodsNum   | 商品编号 | String  | Y    |

##
