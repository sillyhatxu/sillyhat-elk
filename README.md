#docker-compose.yml详解

docker stack deploy -c docker-compose.yml elk

logspout
```
sudo docker run -d --name=logspout --volume=/var/run/docker.sock:/var/run/docker.sock gliderlabs/logspout tcp://172.28.2.25:5000
```
如果希望采集docker信息，则需要发送syslog日志
```
sudo docker run -d --name=logspout --volume=/var/run/docker.sock:/var/run/docker.sock gliderlabs/logspout syslog+tcp://172.16.99.130:5000
```

```
version: '3.3'
networks:
  elk:
services:
  elasticsearch:
    image: 'docker.elastic.co/elasticsearch/elasticsearch:6.1.3'
    networks:
      - elk
    volumes:
      - /home/xushikuan/elasticsearch/data:/usr/share/elasticsearch/data
    ports:
      - '9200:9200'
      - '9300:9300'
    environment:
      - xpack.security.enabled=false
      - "ES_JAVA_OPTS=-Xmx1024m -Xms1024m"

  kibana:
    image: 'docker.elastic.co/kibana/kibana:6.1.3'
    networks:
      - elk
    ports:
      - '5601:5601'
    depends_on:
      - elasticsearch

  logstash:
    image: 'docker.elastic.co/logstash/logstash:6.1.3'
    networks:
      - elk
    ports:
      - '5000:5000'
    environment:
      - DROP_NON_JSON=false
      - STDOUT=true
    volumes:
      - /home/xushikuan/sillyhat-elk/logstash/logstash.conf:/usr/share/logstash/pipeline/logstash.conf
    depends_on:
      - elasticsearch
```