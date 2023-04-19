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
## Тестирование:  
Проще всего оказалось работать с redis через python, поэтому напишем файл redis_db.py, с помощью которого замерим время: 
```
import redis
from time import time
import json


with open('sample1.json') as data_file:
    data = json.load(data_file)

db_test = redis.StrictRedis(
    host='localhost',
    port=6379,
)

# ----------------- add ---------------------------

start = time()
db_test.set('test_json_str', json.dumps(data))
end = time()
print("download string to redis: ", end - start)


start = time()
db_test.hset('test_json_hset', 'test', json.dumps(data))
end = time()
print("download hset to redis: ", end - start)


data_zset = {}
for i, d in enumerate(data):
    data_zset[json.dumps(d)] = i
start = time()
zset = db_test.zadd('test_json_zset', data_zset)
end = time()
print("download zset to redis: ", end - start)


data_list = []
for d in data:
    data_list.append(json.dumps(d))
start = time()
llist = db_test.lpush('test_json_lpush', *data_list)
end = time()
print("download list to redis: ", end - start)

# ------------------- get -------------------------

start = time()
db_test.get('test_json_str')
end = time()
print("get string from redis: ", end - start)


start = time()
db_test.hget('test_json_hset', 'test')
end = time()
print("get hset from redis: ", end - start)


start = time()
db_test.zrangebyscore('test_json_zset', 0, zset)
end = time()
print("get zset from redis: ", end - start)


start = time()
db_test.lrange('test_json_lpush', 0, llist)
end = time()
print("get list from redis: ", end - start)

db_test.flushall()
```
Запускаем и смотрим на результаты:  
<img width="377" alt="Screen Shot 2023-04-19 at 06 12 31" src="https://user-images.githubusercontent.com/71087982/233183029-ff6fac1c-caff-4675-b363-9bcaf2651a14.png">  

