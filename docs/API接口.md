# 常见的 API 接口文档

在 Fuseki 服务器中去尝试一下

- /showtree/:1
 
显示 树 信息

- /knowledge_test/

知识测试

- /login/

登陆

- /register/

注册

- /allpath/

我的个人路径

想办法让这个查询语句运行起来

```
        PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
        PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
        PREFIX owl: <http://www.w3.org/2002/07/owl#>
        PREFIX math:<http://www.semanticweb.org/chengboya/ontologies/%s#>

        FROM <http://www.semanticweb.org/chengboya/ontologies/users/%s%$userid

        SELECT ?father
        WHERE {
            math:%s rdfs:subClassOf ?father.
        }
```
