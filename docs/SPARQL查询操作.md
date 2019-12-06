# SPARQL 查询操作

关于 SPARQL 查询操作主要在```/project/glgl_app/knowledge.py```文件中，从中理解一些常见的查询语句。

首先在 MacOS 系统上安装  jena 命令行工具 ，其他 Linux 系统类似

```
brew install jena
```

## RDF 格式转换

RDF 常见三种格式的数据文件进行相互转换，对于理解 SPARQL 查询语句帮助极大。用 jena 官方的命令行工具 riot 进行 math.ttl (数据文件) 格式相互转换

```
# math.ttl 转换为三元组 math.nt
riot --output=nt math.ttl > math.nt
```

## 执行查询

从 math.nt 文件，三元组去理解 SPARQL 查询语句，启动 fuseki 服务器

```
# 加载 dataset 目录下的数据
fuseki-server --loc=dataset /dataset
```

### 没有前缀的查询语句

```
SELECT ?p 
where{
<http://www.semanticweb.org/chengboya/ontologies/math#闭区间上连续函数的性质> <http://www.semanticweb.org/chengboya/ontologies/math#description> ?p .
}
```

输出结果 :

```
p	
"介值定理，有界性，最大值最小值定理，零点定理"
```

如果增加前缀，可以简化查询语句的：

```
PREFIX math:<http://www.semanticweb.org/chengboya/ontologies/math#>
SELECT ?p
where{
        math:闭区间上连续函数的性质 math:description ?p .
}
```

与上面的输出结果相同的，这里也表示前缀的作用。

### 找到知识的先修知识点

```
PREFIX math:<http://www.semanticweb.org/chengboya/ontologies/math#>
SELECT ?p
where{
        ?p math:sk_is math:积分表 .
}
```

输出结果:

```
p	
math:不定积分的概念与性质
```

### 找到视频对应的知识点

```
PREFIX math:<http://www.semanticweb.org/chengboya/ontologies/math#>
SELECT ?p
where{
        ?p math:resource math:id_18 .
}
```

输出结果:

```
p
math:math
math:函数	
math:函数的极限
```

### math的default graph 不考虑视频难度，筛选出质量TOP10视频资源

```
PREFIX math:<http://www.semanticweb.org/chengboya/ontologies/math#>

SELECT ?resource ?quality_value
WHERE {
        math:函数 math:resource ?resource.
        ?resource math:quality_is ?quality_value
}
ORDER BY DESC(?quality_value)
LIMIT 10
```

输出结果 :

```
resource	            quality_value	
math:id_86              "0.380000"^^xsd:decimal
math:id_100             "0.350000"^^xsd:decimal
math:id_67              "0.090000"^^xsd:decimal
math:id_76              "0.030000"^^xsd:decimal
math:id_75              "0.020000"^^xsd:decimal
math:id_84              "0.020000"^^xsd:decimal
math:id_103             "0.010000"^^xsd:decimal
math:id_12              "0.010000"^^xsd:decimal
math:id_24              "0.010000"^^xsd:decimal
math:id_26              "0.010000"^^xsd:decimal
```

### math的default graph 考虑视频难度，筛选出与用户掌握程度匹配的质量TOP10视频资源，用于学习方案的制定

```
PREFIX math:<http://www.semanticweb.org/chengboya/ontologies/math#>

SELECT ?resource ?quality_value

WHERE {
        math:函数 math:resource ?resource.
        ?resource math:difficulty_is ?difficulty_value
        FILTER( ?difficulty_value = 0.200000)
        ?resource math:quality_is ?quality_value
}
ORDER BY DESC(?quality_value)
```

输出结果:

```
resource	     quality_value	
math:id_75       "0.020000"^^xsd:decimal
math:id_84       "0.020000"^^xsd:decimal
math:id_12       "0.010000"^^xsd:decimal
math:id_24       "0.010000"^^xsd:decimal
math:id_45       "0.010000"^^xsd:decimal
math:id_14       "0.000000"^^xsd:decimal
math:id_21       "0.000000"^^xsd:decimal	
math:id_28       "0.000000"^^xsd:decimal	
math:id_37       "0.000000"^^xsd:decimal	
math:id_39       "0.000000"^^xsd:decimal	
math:id_62       "0.000000"^^xsd:decimal
math:id_63       "0.000000"^^xsd:decimal
math:id_72       "0.000000"^^xsd:decimal
math:id_81       "0.000000"^^xsd:decimal	
math:id_87       "0.000000"^^xsd:decimal
math:id_90       "0.000000"^^xsd:decimal
math:id_93       "0.000000"^^xsd:decimal
```

### math的default graph 不考虑视频难度，筛选出质量TOP10视频资源

```
PREFIX math:<http://www.semanticweb.org/chengboya/ontologies/math#>

SELECT ?resource ?quality_value

WHERE {
        math:函数 math:resource ?resource.
        ?resource math:quality_is ?quality_value
}
ORDER BY DESC(?quality_value)
LIMIT 10
```
运行结果略

### math 查询某一视频的难度 （默认一个视频对应一个知识点，但一个知识点可以对应多个视频资源实例）

```
PREFIX math:<http://www.semanticweb.org/chengboya/ontologies/math#>

SELECT ?difficulty

WHERE {
        math:id_66 math:difficulty_is ?difficulty.
}
```
运行结果略

### default graph 、named graph 均可以。没有用FROM 找知识点的子知识点的学习起点

```
PREFIX math:<http://www.semanticweb.org/chengboya/ontologies/math#>

SELECT ?start

WHERE {
        math:函数 math:beginningwith ?start.
}
```
运行结果略

### rdfs:subClassOf 找到子类知识点

```
PREFIX math:<http://www.semanticweb.org/chengboya/ontologies/math#>

SELECT ?start

WHERE {
        math:函数的基本求导法则 rdfs:subClassOf ?start .
}
```
这里报错了，rdfs 没有定义的，```<http://www.w3.org/2000/01/rdf-schema#subClassOf>```，实际上这个的，要增加这个的。

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
```

最终的代码部分:

```
PREFIX math:<http://www.semanticweb.org/chengboya/ontologies/math#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?start

WHERE {
        math:函数的基本求导法则 rdfs:subClassOf ?start .
}
```

运行结果 :

```
start	
math:函数的导数
```

### owl:equivalentClass 找到同等知识点

```
PREFIX math:<http://www.semanticweb.org/chengboya/ontologies/math#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>


SELECT ?start

WHERE {
        math:函数的基本求导法则 owl:equivalentClass ?start .
}
```

运行结果:

```
start	
math:利用求导法则
```














