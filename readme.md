# KG项目部署说明文档
## Zip包解压
### 解压到当前目录
```python
unzip kg_proj.zip
```

### 解压后得到两个文件夹：
- qm00066_neo4j - Neo4j 

- qm00066_kgbackend - 后端服务文件

## Docker 启动命令
### 加载neo4j镜像
```python
docker load -i qm00066_neo4j.tar
```

### 启动neo4j
```python
docker run -d \
    --name=neo4j \
    -p 7474:7474 \
    -p 7687:7687 \
    -v /mnt/wubotao/neo4j/data:/data \
    -v /mnt/wubotao/neo4j/logs:/logs \
    -v /mnt/wubotao/neo4j/import:/var/lib/neo4j/import \
    -v /mnt/wubotao/neo4j/plugins:/plugins  \
    -e NEO4J_AUTH=neo4j/quantum2018 \
    -e NEO4J_PLUGINS='["apoc"]' \
    --restart always \
    --privileged \
    qm00066_neo4j:latest
```

### neo4j可视化
- http://121.37.118.88:7474/
- neo4j/quantum2018

### 加载后端镜像\
```python
docker load -i qm00066_kgbackend.tar
```

### 启动后端
```python
docker run -itd --name=backend  \
    --volume /mnt/wubotao/projects/changsha_nudt_kg_algrithms:/mnt \
    --workdir=/mnt -p 5000:5000 \
    --restart=always \
    --privileged \
    qm00066_kgbackend:latest 
```

```python
docker exec -it test_backend bash \
python app_server.py
```

### 后端服务可视化
- http://121.37.118.88:5000/docs