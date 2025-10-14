Run the following to start the interactive Postgres CLI:
```bash
docker exec -i -t postgres psql --username postgres  -p 5432 -h localhost --no-password
```

At the `admin=#` prompt, change to the `orders`table:
```bash
\c orders;
```

At the `orders=#` prompt, select all rows:
```bash
select * from orders;
SELECT device.id,device.sn,device.uid,device.name,device.status,device.login,device.online,device.hw_version,device.product_key,device.updatedat,device.createdat,device.iot_platform_param FROM device WHERE sn = 'KA42500090DXI'
```



# 数据库操作
```bash
# 进入容器后，选择一个数据库并进入postgre中
psql -d dbname

# 进入指定的数据库中
\c dbname

# 查看所有的数据库
\l

# 查看该数据库下所有的表
\d

# 查看表格信息
\d tablename

```


## JSON类型和函数
postgres支持两种json数据类型：json和jsonb

### 不同点
- json
	- 效率：对输入完整拷贝，使用时再去解析，会保留输入的空格，重复键以及顺序等。
	- 存储快，使用慢
- jsonb
	- 效率：解析输入后保存的二进制，他在解析时会删除不必要的空格和重复的键，顺序和输入可能也不相同。使用时不用再次解析。
	- 存储稍慢，使用较快
	- 支持索引

### 相同点
- 两者对重复键的处理都是保留最后一个键值对。
- 键值对的键必须使用双引号


- `("UPDATE device SET config = jsonb_set('{}','{%s}' ,%s,true)WHERE sn =  '%s';", number, values, devicekey)`
	- json字段内容为空时，新增一个key，value
- `("UPDATE device SET config = jsonb_set(cast((select config from device where sn " + "= '%s') as jsonb),'{%s}' ,%s,true)WHERE sn =  '%s';", devicekey, number, values, devicekey)`
	- json字段内容不为空时，新增一个key，value
- `UPDATE device SET config = config::jsonb - '%s' WHERE sn = '%s';", number, devicekey`
	- 删除json字段中指定key
# 进入kubeSphere的postgre数据库的方式

`psql -U admin -h10.233.74.122 -p5432 -dseanet`
- -U  用户名
- -h   数据库IP
- -p   数据库端口
- -d   需要访问的数据库名称
- 再输入密码 111111

seanet线上： psql -U seanet -h192.168.15.70 -p5432 -dseanet       s1g0h7!Aj


## 数据库中数据

iot_platform
```json
{

    "status": true,

    "code": 0,

    "message": "成功",

    "data": {

        "total": 5,

        "current": 1,

        "per_page": 20,

        "size": 5,

        "payload": [

            {

                "id": 21,

                "name": "mqtt平台",

                "platform_type": "mqtt",

                "platform_config": {},

                "updatedat": "2022-09-06T04:37:10.717177Z",

                "createdat": "2022-09-01T17:53:50.978857Z"

            },

            {

                "id": 28,

                "name": "腾讯云平台",

                "platform_type": "tencent",

                "platform_config": {},

                "updatedat": "2022-09-06T04:37:20.058972Z",

                "createdat": "2022-09-02T18:27:32.148144Z"

            },

            {

                "id": 29,

                "name": "阿里云平台",

                "platform_type": "aliyun",

                "platform_config": {},

                "updatedat": "2022-09-06T04:37:26.352969Z",

                "createdat": "2022-09-03T13:57:00.02185Z"

            },

            {

                "id": 37,

                "name": "测试云平台",

                "platform_type": "tencent",

                "platform_config": {},

                "updatedat": "2022-09-06T04:37:02.853997Z",

                "createdat": "2022-09-05T03:27:52.747937Z"

            },

            {

                "id": 41,

                "name": "iot.netkit.cloud",

                "platform_type": "netkit",

                "platform_config": {

                    "addr": "iot.netkit.cloud",

                    "password": "Sealan.tech",

                    "port": "1883",

                    "projectKey": "PRJbXZerk",

                    "tls": false,

                    "username": "sealan"

                },

                "updatedat": "2022-11-25T10:42:36.406174Z",

                "createdat": "2022-10-31T09:49:57.110364Z"

            }

        ]

    }

}
```

product
```json
{

    "status": true,

    "code": 0,

    "message": "成功",

    "data": {

        "total": 7,

        "current": 1,

        "per_page": 20,

        "size": 7,

        "payload": [

            {

                "id": 5,

                "name": "测试云产品",

                "product_key": "Nv1hMzN0kN",

                "iot_platform_id": 37,

                "iot_platform_config": {},

                "updatedat": "2022-09-06T04:37:48.037609Z",

                "createdat": "2022-09-06T04:37:48.037609Z"

            },

            {

                "id": 6,

                "name": "测试云产品",

                "product_key": "C93v5MCmRh",

                "iot_platform_id": 37,

                "iot_platform_config": {},

                "updatedat": "2022-09-06T07:12:50.423016Z",

                "createdat": "2022-09-06T04:37:57.524458Z"

            },

            {

                "id": 7,

                "name": "测试mqtt产品",

                "product_key": "A6FCJBYUvW",

                "iot_platform_id": 28,

                "iot_platform_config": {},

                "updatedat": "2022-09-06T07:58:13.380275Z",

                "createdat": "2022-09-06T04:43:52.679678Z"

            },

            {

                "id": 8,

                "name": "测试云产品2",

                "product_key": "ada970",

                "iot_platform_id": 41,

                "iot_platform_config": {},

                "updatedat": "2022-10-31T09:54:06.501025Z",

                "createdat": "2022-10-31T09:54:06.501025Z"

            },

            {

                "id": 9,

                "name": "seanetProduct",

                "product_key": "Rx6Rx1",

                "iot_platform_id": 41,

                "iot_platform_config": {

                    "dataformat": "nqproto",

                    "netkitProductKey": "59gZXv",

                    "nodetype": 1,

                    "protocol": "mqtt"

                },

                "updatedat": "2022-12-21T10:19:54.762425Z",

                "createdat": "2022-11-10T11:53:34.492623Z"

            },

            {

                "id": 11,

                "name": "seanetIR4",

                "product_key": "ir4",

                "iot_platform_id": 41,

                "iot_platform_config": {

                    "dataformat": "nqproto",

                    "netkitProductKey": "79VY3v",

                    "nodetype": 1,

                    "protocol": "mqtt"

                },

                "updatedat": "2022-12-27T10:56:18.526569Z",

                "createdat": "2022-12-27T10:56:18.526569Z"

            },

            {

                "id": 12,

                "name": "seanet IR4",

                "product_key": "HcIqSabhj7",

                "iot_platform_id": 41,

                "iot_platform_config": {

                    "dataformat": "nqproto",

                    "netkitProductKey": "bxZnbx",

                    "nodetype": 1,

                    "protocol": "mqtt"

                },

                "updatedat": "2022-12-27T11:48:48.520112Z",

                "createdat": "2022-12-27T11:48:48.520112Z"

            }

        ]

    }

}
```

device
```json
{

    "status": true,

    "code": 0,

    "message": "成功",

    "data": {

        "total": 4,

        "current": 1,

        "per_page": 20,

        "size": 4,

        "payload": [

            {

                "id": 2,

                "sn": "test device sn",

                "uid": "",

                "name": "",

                "status": 0,

                "product_key": "A6FCJBYUvW",

                "updatedat": "2022-09-07T11:05:11.431638Z",

                "createdat": "2022-09-07T11:05:11.431638Z",

                "iot_platform_param": null

            },

            {

                "id": 3,

                "sn": "J81140019RGPZ",

                "uid": "",

                "name": "",

                "status": 0,

                "product_key": "ada970",

                "updatedat": "2022-10-31T10:01:24.588374Z",

                "createdat": "2022-10-31T10:01:24.588374Z",

                "iot_platform_param": {

                    "secret": "F34gVbhty1ppwXhh"

                }

            },

            {

                "id": 4,

                "sn": "869516057017852",

                "uid": "testDevice",

                "name": "",

                "status": 0,

                "product_key": "Rx6Rx1",

                "updatedat": "2022-11-10T11:54:57.88755Z",

                "createdat": "2022-11-10T11:54:57.88755Z",

                "iot_platform_param": {

                    "secret": "l0QxBdgfx13z0eyz"

                }

            },

            {

                "id": 7,

                "sn": "KA42500090DXI",

                "uid": "testDevice",

                "name": "seanetIR4",

                "status": 0,

                "product_key": "ir4",

                "updatedat": "2022-12-27T10:57:27.625661Z",

                "createdat": "2022-12-27T10:57:27.625661Z",

                "iot_platform_param": {

                    "secret": "KpiWEdaJ7xGEzCvY"

                }

            }

        ]

    }

}
```