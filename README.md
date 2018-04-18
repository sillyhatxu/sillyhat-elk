#docker-compose.yml详解

标准配置文件应该包含 version、services、networks 三大部分，其中最关键的就是 services 和 networks 两个部分，下面先来看 services 的书写规则。
##version
版本对应关系：Please read [compose-file](https://docs.docker.com/compose/compose-file/)

![image](https://github.com/xushikuan/sillyhat-elk/tree/master/file/docker-compose/compose-file-version.png)
![image](https://github.com/xushikuan/sillyhat-elk/raw/master/file/docker-compose/compose-file-version.png)

```
version: '3.3'
networks:
  elk:
services:
  logstash:
    image: 'docker.elastic.co/logstash/logstash:6.1.3'
    networks:
      - elk
    ports:
      - '5000:5000'
    environment:
      - DROP_NON_JSON=false
      - STDOUT=true
    depends_on:
      - elasticsearch

  kibana:
    image: 'docker.elastic.co/kibana/kibana:6.1.3'
    networks:
      - elk
    ports:
      - '5601:5601'
    depends_on:
      - elasticsearch

  elasticsearch:
    image: 'docker.elastic.co/elasticsearch/elasticsearch:6.1.3'
    networks:
      - elk
    volumes:
      - /home/xushikuan/data/elasticsearch:/usr/share/elasticsearch/data
    ports:
      - '9200:9200'
      - '9300:9300'
    environment:
      - xpack.security.enabled=false
      - "ES_JAVA_OPTS=-Xmx1024m -Xms1024m"
```