# docker安装neo4j

```shell
docker pull neo4j:3.3.1 #安装neo4j,版本为3.3.1,测试有效

docker run -d --name neo4j -p 7474:7474 -p 7687:7687 -v /home/xiaohui/Downloads/neo4j/data:/data -v /home/xiaohui/Downloads/neo4j/logs:/logs -v /home/xiaohui/Downloads/neo4j/conf:/var/lib/neo4j/conf -v /home/xiaohui/Downloads/neo4j/import:/var/lib/neo4j/import --env NEO4J_AUTH=none neo4j:3.3.1 #启动docker容器
```

## 导入rdf数据

```shell
docker exec -it neo4j bash # 进入容器

# 该版本docker容器使用的是Alpine Linux,使用apk命令安装软件

apk add vim # 安装 vim

# 下载neosemantics jar包，将jar复制到neo4j/plugins目录下
wget https://github.com/jbarrasa/neosemantics/releases/download/3.3.0.2/neosemantics-3.3.0.2.jar
mv neosemantics-3.3.0.2.jar /var/lib/neo4j/plugins/

# 在neo4j/neo4j.conf文件中添加以下内容：
dbms.unmanaged_extension_classes=semantics.extension=/rdf

# 重启neo4j服务
exit
docker restart neo4j
```

访问neo4j web端，在浏览器中输入http://localhost:7474

1 创建常规标签，这是导入数据必要步骤

在浏览器中输入如下命令

CREATE INDEX ON:Resource(uri)

CREATE INDEX ON:URI(uri)

CREATE INDEX ON:BNode(uri)

CREATE INDEX ON:Class(uri)

2 CALL semantics.importRDF('{RDF文件路径}','Turtle',{languageFilter: 'en'})，输入该命令导入RDF文件至neo4j

例如 CALL semantics.importRDF('file:///home/rdf.ttl','Turtle',{languageFilter: 'en'})
