## 运行程序

- windows：直接双击运行
- linux：`chmod  +x aiotdb-linux | bash`

运行后浏览器访问：<b style="color:red">8567</b> 端口 或者直接 <b style="color:red">在线体验：http://aiotdb.net:8567</b>



### 查询性能

极致性能，取决于服务器配置。<b style="color:red">毫秒级别查询！！！读写不锁</b>



## 快速入门

<b style="color:red">直接浏览器访问</b>

### 写入数据

```api
http://127.0.0.1:8567/v1/execute?sql=insert into `t01.p01.a01.d01.p01` VALUES (1663315210000, '10'),(1663315220000, '11'),(1663315230000, '12'),(1663315240000, '11'),(1663315250000, '10'),(1663315260000, '18'),(1663315270000, '14'),(1663315280000, '12'),(1663315290000, '19'),(1663315300000, '11')
```

### 查询数据

- 普通查询

```api
http://127.0.0.1:8567/v1/query?sql=select * from `t01.p01.a01.d01.p01` where ts>0 order by ts desc limit 10
```

- 附近查询

```api
http://127.0.0.1:8567/v1/query?sql=select * from `t01.p01.a01.d01.p01` where `ts` near (16627392922001,16627392921001)
```

### 删除数据

```api
http://127.0.0.1:8567/v1/execute?sql=delete from `t01.p01.a01.d01.p01`%20where%20`ts`=1663315250000
```

### 数据统计

```api
http://127.0.0.1:8567/v1/query?sql=select count(*) from `t01.p01.a01.d01.p01` where `ts`>0
```

### 可视化展示

```api
# svg显示
http://127.0.0.1:8567/v1/draw/svg?sql=select * from `t01.p01.a01.d01.p01` where ts>0 order by ts desc limit 10

# echarts显示
http://127.0.0.1:8567/v1/draw/echarts?sql=select * from `t01.p01.a01.d01.p01` where ts>0 order by ts desc limit 10
```

### 时间线查询

```api
http://127.0.0.1:8567/v1/query?sql=show `t01.p01.**`
```

### 健康检查

```api
http://127.0.0.1:8567/node/health
```

### 服务器状态

```api
http://127.0.0.1:8567/node/host
```