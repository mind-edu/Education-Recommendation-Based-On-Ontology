# 登录

首页用FeanLau和134026Fean. 登录

http://127.0.0.1:8000/admin/ 后台也可以登录的


Django administration  后台是可以修改用户名和密码的

```
python3 manage.py dumpdata > datadump.json
```

导出数据到json


## 运行本体的展示

```
        self.sparql = SPARQLWrapper("http://localhost:3030/%s/sparql" % dataset,
```

对的，端口号是3000， 这个怎么运行的啊


运行 Fuseki 的服务器的，dataset就是数据的

### 事先将三元组数据导入tdb

```
但是现在的Fuseki的数据已经已经生成了，怎么办？
tdbloader --loc tdb kg_demo_movie.nt

也许是这个？

# 这个启动成功了
fuseki-server --loc=dataset /dataset
```

拿到了这里的math.ttl 然后导入成功，从后端的Fuseki拿到了数据成功了


这个项目的启动需要两种服务器的，一个是3030的Fuseki，一个是http://127.0.0.1:8000的Python Web端