# Redis DB  
Сначала запустим Redis с помощью простенького docker-compose.yaml файла с лекции на одной ноде: 
```
version: "3.8"

networks:
    redis_network:
        driver: bridge

volumes:
 rdb:
    name: rdb
 rdbcfg:
    name: rdbcfg
 rtdb:
    name: rtdb

services:
  redisdb:
    image: redis
    container_name: redisdb
    ports:
      -  6379:6379
    volumes:
      -  rdb:/var/lib/redis
      -  rdbcfg:/usr/local/etc/redis/redis.conf
    environment:
      -  REDIS_REPLICATION_MODE=master
    networks:
      -  redis_network
  
  redisinsight:
    image: redislabs/redisinsight
    container_name: redisinsight
    ports:
      -  '8001:8001'
    volumes:
      -  rtdb:/db
    networks:
      -  redis_network
```  
Запустим докер с помощью команды anymax$ docker-compose -f docker-compose.yml up -d. Затем проверим, что все корректно работает с помощью такой команды:  
<img width="281" alt="Screen Shot 2023-04-19 at 04 50 03" src="https://user-images.githubusercontent.com/71087982/233181914-fe11fc1e-7482-4cbb-b38d-475d61132244.png">  
Ответ получен, значит все ок

