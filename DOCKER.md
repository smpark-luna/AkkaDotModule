# AKKA TOOL Test를 위한 인프라 구동


# 
    Compose> docker-compose up -d



# docker-compose.yml

    version: '3.4'
 
    services:
      mysql:
        image: mysql:5.7
        command: --default-authentication-plugin=mysql_native_password
        restart: always
        ports:
          - 13306:3306    
        environment:
          MYSQL_ROOT_PASSWORD: "root"
          TZ: "Asia/Seoul"

      postgres:
        image: postgres:9.6
        container_name: postgres
        environment:
          - POSTGRES_USER=docker
          - POSTGRES_PASSWORD=docker        
        ports:
          - '5432:5432'
        volumes:
          - postgre-data:/usr/local/psql/data
      
      redis_auth:
        image: bitnami/redis:5.0
        environment:
         - ALLOW_EMPTY_PASSWORD=yes
        ports:
         - 7000:6379
      zookeeper:
        image: 'bitnami/zookeeper:latest'
        ports:
        - '2181:2181'
        environment:
        - ALLOW_ANONYMOUS_LOGIN=yes
        labels:
            io.rancher.scheduler.affinity:host_label: platform=etc01
            io.rancher.container.hostname_override: container_name   
      kafka:
        hostname: kafka
        image: 'bitnami/kafka:latest'
        ports:
        - '9092:9092'
        environment:
        - KAFKA_ADVERTISED_HOST_NAME=kafka
        - KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181
        - ALLOW_PLAINTEXT_LISTENER=yes

    volumes:
      elasticsearch-data:
        driver: local
      postgre-data:
        driver: local
        
