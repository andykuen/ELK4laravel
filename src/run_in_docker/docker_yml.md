# 設定 docker.yml
## .env
```
APP_NAME=web
APP_PORT=8888

ELK_VERSION=6.4.0

ES_HOST=web-elasticsearch
ES_PORT1=9200
ES_PORT2=9300

LS_HOST=web-logstash
LS_PORT1=5000
LS_PORT2=9600

KB_HOST=web-kibana
KB_PORT=5601
```
## docker-compose.yml
```
version: "3.5"
services:
  web:
    container_name: ${APP_NAME}
    hostname: ${APP_NAME}
    network_mode: web_LAN
    build:
      context: ./
      dockerfile: Dockerfile
      args:
        - LS_HOST=${LS_HOST}:${LS_PORT1}
    ports:
      - ${APP_PORT}:8080
    volumes:
      - ./:/var/www/html/
    environment:
      DB_HOST: ${DB_HOST}
      REDIS_HOST: ${REDIS_HOST}
    depends_on:
      - logstash

  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:${ELK_VERSION}
    container_name: ${ES_HOST}
    hostname: ${ES_HOST}
    ports:
      - "${ES_PORT1}:9200"
      - "${ES_PORT2}:9300"
    environment:
      ES_JAVA_OPTS: "-Xms256m -Xmx256m"
    network_mode: apim2_LAN

  logstash:
    image: docker.elastic.co/logstash/logstash:${ELK_VERSION}
    container_name: ${LS_HOST}
    hostname: ${LS_HOST}
    ports:
      - "${LS_PORT1}:5000"
      - "${LS_PORT2}:9600"
    network_mode: apim2_LAN
    depends_on:
      - elasticsearch
    environment:
      LS_JAVA_OPTS: "-Xms256m -Xmx256m"
      ES_HOST: ${ES_HOST}
    volumes:
      - ./ELK/logstash/pipeline:/usr/share/logstash/pipeline:ro
      - ./ELK/logstash/config/logstash.yml:/usr/share/logstash/config/logstash.yml:ro
      - ./storage/logs/:/home/inputFile
      # - ./ELK/logstash/inputFile/:/home/inputFile
      # - ./ELK/logstash/DAO/:/home/DAO


  kibana:
    image: docker.elastic.co/kibana/kibana:${ELK_VERSION}
    container_name: ${KB_HOST}
    hostname: ${KB_HOST}
    ports:
      - "${KB_PORT}:5601"
    network_mode: apim2_LAN
    depends_on:
      - elasticsearch
    environment:
      SERVER_NAME: ${KB_HOST}
      ELASTICSEARCH_URL: http://${ES_HOST}:${ES_PORT1}
```
## docker-compose.yml
```
FROM 1and1internet/ubuntu-16-apache-php-7.2

EXPOSE 8080

WORKDIR /var/www/html/public
```