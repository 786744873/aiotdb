## 数据接入方式

目前支持两种接入方式

- MQTT
- Http



### MQTT接入

> Mqtt broker 代理服务端口号默认：<b style="color:red">1883</b>

#### <span id="mqtt_point">点存储</span>

- **topic：**`point`

- **payload：**

  ```json
  {
    "name": "{name为测点信息}",
    "values":{
      "{测点毫秒时间戳01}":"{测点值01}",
      "{测点毫秒时间戳02}":"{测点值02}"
    }
  }
  ```

- 示例数据

  ```json
  {
    "name": "t01.p01.a01.d01.p01",
    "values":{
      "1663315220055":"0.1234",
      "1663315220056":"0.4567"
    }
  }
  ```

  



#### <span id="mqtt_device">设备存储</span>

- **topic：**`device`

- **payload：**

  ```json
  {
  	"name": "{name为测点信息}",
  	"values": {
  		"{测点01}": {
  			"{测点毫秒时间戳01}": "{测点值01}",
  			"{测点毫秒时间戳02}": "{测点值02}"
  		},
  		"{测点02}": {
  			"{测点毫秒时间戳02}": "{测点值01}",
  			"{测点毫秒时间戳03}": "{测点值02}"
  		}
  	}
  }
  ```

- 示例数据

  ```json
  {
  	"name": "t01.p01.a01.d01",
  	"values": {
  		"p01": {
  			"1663315220055": "0.1234",
  			"1663315220056": "0.2345"
  		},
  		"p02": {
  			"1663315220057": "0.5678",
  			"1663315220058": "0.6789"
  		}
  	}
  }
  ```



### Http接入

> Http接入服务端口号默认：<b style="color:red">8567</b>

#### AIoT-SQL引擎说明

> 注意：
>
> 所有的数据库名称、数据字段名称,统一使用 ` 分割符号进行包裹，否则无法识别
>
> sql引擎不支持;符号

<b style="color:red">MySQL语法</b>基本上都会吧，兼容大部分MySQL查询语句

#### 存储API

- > 支持批量存储

  ```shell
  # 最大支持100条数据批量写入
  GET     /v1/execute?sql={sql}
  
  # 最大支持1万条数据写入，建议最多5000
  POST    /v1/execute
  Content-Type: application/json'
  {
  	"sql":"{sql}"
  }
  ```

  

- 示例数据：

  ```shell
  curl --location --request POST 'http://127.0.0.1:8567/v1/execute' \
  --header 'Content-Type: application/json' \
  --data-raw '{
      "sql": "INSERT INTO `t01.p01.a01.d01.p01` VALUES (16627392970000, '\''2509.5506'\''),(16627392971000, '\''2509.5506'\''),(16627392972000, '\''2509.5506'\'')"
  }'
  ```

  

#### 查询API

> 支持条件筛选与排序

#### 查询时序数据

-  请求说明

  ```shell
  GET     /v1/query?sql={sql}
  
  POST    /v1/query
  Content-Type: application/json'
  {
  	"sql":"{sql}"
  }
  ```

- 示例数据：

  ```shell
  curl --location --request GET 'http://127.0.0.1:8567/v1/query?sql=select * from `t01.p01.a01.d01.p01` where `ts`>  166273926100 order by `ts` desc  limit 10'
  ```

- 查询结果：

  ```json
  {
      "code": 200,
      "msg": "操作成功",
      "data": [
          {
              "ts": 1664515379000,
              "value": "452"
          },
          {
              "ts": 1664515380000,
              "value": "467"
          },
          {
              "ts": 1664515381000,
              "value": "654"
          }
      ]
  }
  ```

  

#### 统计时序数量

- 请求说明

  ```shell
  GET     /v1/query?sql={sql}
  
  POST    /v1/query
  Content-Type: application/json'
  {
  	"sql":"{sql}"
  }
  ```

- 示例数据：

  ```shell
  curl --location --request GET 'http://127.0.0.1:8567/v1/query?sql=select count(*) from `t01.p01.a01.d01.p01` where `ts`>  166273926100'
  ```

- 查询结果：

  ```json
  {
      "code": 200,
      "msg": "操作成功",
      "data": 3
  }
  ```



#### 时间序列查询

  <b style="color:red">`*`</b>表示一级时间序列（可省略），<b style="color:red">`**`</b>表示无限级时间序列

-  请求说明

  ```shell
  GET     /v1/query?sql={sql}
  
  POST    /v1/query
  Content-Type: application/json'
  {
  	"sql":"{sql}"
  }
  ```

- 示例数据：

  ```shell
  # 查询一级时间序列
  curl --location --request GET 'http://127.0.0.1:8567/v1/query?sql=show `t01.p01.a01`'
  curl --location --request GET 'http://127.0.0.1:8567/v1/query?sql=show `t01.p01.a01.*`'
  # 查询所有时间序列
  curl --location --request GET 'http://127.0.0.1:8567/v1/query?sql=show `t01.p01.a01.**`'
  ```

- 查询结果：

  ```shell
  # 查询一级时间序列
  # curl --location --request GET 'http://127.0.0.1:8567/v1/query?sql=show `t01.p01.a01.*`'
  # curl --location --request GET 'http://127.0.0.1:8567/v1/query?sql=show `t01.p01.a01.*`'
  {
      "code": 200,
      "msg": "操作成功",
      "data": [
          "t01.p01.a01.d01.*",
          "t01.p01.a01.d02.*"
      ]
  }
  
  # 查询所有时间序列
  # curl --location --request GET 'http://127.0.0.1:8567/v1/query?sql=show `t01.p01.a01.**`'
  {
      "code": 200,
      "msg": "操作成功",
      "data": [
          "t01.p01.a01.*",
          "t01.p01.a01.d01.*",
          "t01.p01.a01.d01.23",
          "t01.p01.a01.d01.24",
          "t01.p01.a01.d01.*",
          "t01.p01.a01.d02.22"
      ]
  }
  ```