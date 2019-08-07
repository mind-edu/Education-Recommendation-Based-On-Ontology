# Education-Recommendation-Based-On-Ontology
以本体结构为基础，组织管理教育资源；利用本体技术创建用户知识体系，根据用户的自身学习情况，为其推荐教育视频资源。

推荐的形式有两种：

1.学习方案：展示用户所有知识点的掌握情况并对各个知识点进行资源的推荐；

2.学习路径：以用户正在学习的知识点为中心，结合知识体系，生成一条学习路径。该路径包括用户应该学习该中心知识点的前置知识点和后续知识点。


```
pip3 install -r requirements.txt
```


```
python3 manage.py migrate
```

出现的结果如下：

```
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, glgl_app, sessions
Running migrations:
  No migrations to apply.
```



迁移数据库

在上一步所在的位置运行如下命令迁移数据库：

python manage.py migrate
创建后台管理员账户

在上一步所在的位置运行如下命令创建后台管理员账户

python manage.py createsuperuser
具体请参阅 在 Django Admin 后台发布文章

运行开发服务器

在上一步所在的位置运行如下命令开启开发服务器：

python manage.py runserver
在浏览器输入：127.0.0.1:8000

这个项目能够运行起来的


## math.ttl
 
根据 math.ttl 很容易看出来具体的数据的，owl文件只是本体的结构，然后每个节点上绑定数据的

