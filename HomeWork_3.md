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
Запустим докер с помощью команды docker-compose -f docker-compose.yml up -d. Затем проверим, что все корректно работает с помощью такой команды:  
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
Интересно, что время заполнения и получения значения по ключю одинаковое примерно  
## Redis на трех нодах  
Потратив немало часов на поиски файла docker-compose.yml, который будет работать, получаем такой файл:  
```
version: '3.7'
services:
  master:
    image: redis
    container_name: redis-master
    restart: always
    command: redis-server --port 6379 --appendonly yes
    ports:
      - 6379:6379
    volumes:
      - ./data:/data
 
  slave1:
    image: redis
    container_name: redis-slave-1
    restart: always
    command: redis-server --slaveof 192.168.8.188 6379 --port 6380   --appendonly yes
    ports:
      - 6380:6380
    volumes:
      - ./data:/data
 
 
  slave2:
    image: redis
    container_name: redis-slave-2
    restart: always
    command: redis-server --slaveof 192.168.8.188 6379 --port 6381  --appendonly yes
    ports:
      - 6381:6381
    volumes:
      - ./data:/data
```  
Здесь как раз содаются 3 ноды для Redis - один мастер и две slave  
Запускаем с помощью команды docker-compose -f docker-compose.yml up -d и проверяем, что все работает на нашем порту:  
<img width="575" alt="Screen Shot 2023-04-19 at 22 28 08" src="https://user-images.githubusercontent.com/71087982/233183904-8a94e2cb-1609-4f7a-a569-0136317148b0.png">  
## Тестирование
Теперь с помощью того же файла redis_db.py делаем замеры времени:  
<img width="406" alt="Screen Shot 2023-04-19 at 22 31 19" src="https://user-images.githubusercontent.com/71087982/233184063-e0231803-224f-4bc3-a436-6fc42264bd68.png">  
Стало работать дольше. Это произошло из за того что теперь надо поддерживать связь с тремя нодами, а на коммуникацию уходит время. При этом теперь время добавления и получения отличаются сильнее, чем на одной ноде. Но теперь надежность повысилась

