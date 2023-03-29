# MongoDB  
Для начала необходимо скачать Docker и Mongo Compass.  
Далее в папке, в которой я работаю, создала файл docker-compose.yml:  
<img width="519" alt="Screen Shot 2023-03-29 at 07 07 00" src="https://user-images.githubusercontent.com/71087982/228604642-83264cae-fe1a-4f1a-a7d1-767c9aeb4297.png">  
Затем запустила докер через команду:  
<img width="582" alt="Screen Shot 2023-03-29 at 07 06 46" src="https://user-images.githubusercontent.com/71087982/228605264-596fe46e-a4a2-4554-9345-a34a8dbd4340.png">  
Проверила, что все работает, в докере появился наш образ:  
<img width="1267" alt="Screen Shot 2023-03-29 at 07 06 15" src="https://user-images.githubusercontent.com/71087982/228605519-a2ff6e63-090e-4f88-9900-ef5bed0ce656.png">
В Mongo Compose так же получилось подключиться по локальному хосту 27017:27017:  
<img width="1415" alt="Screen Shot 2023-03-29 at 07 06 23" src="https://user-images.githubusercontent.com/71087982/228605784-4519448f-996b-470b-8e38-8574cfa1e2d0.png">
Все заработало, значит можно создавать базу данных!  Создала через: use fast food и db.createCollection('american-fast-food'). 
<img width="353" alt="Screen Shot 2023-03-29 at 09 54 08" src="https://user-images.githubusercontent.com/71087982/228606251-38b96887-bf4f-4e57-aeb1-0d30c80b3eb6.png">  
Взяла уже готовую базу данных ([с сайта](https://www.kaggle.com/datasets/thedevastator/fast-food-restaurants-in-the-united-states?resource=download)) и загрузила ее через интерфейс программы Mongo Compose:  
 <img width="592" alt="Screen Shot 2023-03-29 at 09 53 51" src="https://user-images.githubusercontent.com/71087982/228606822-cdaac8e8-8fc2-411b-b3af-a4a41f6b771b.png">
Проверила, что загрузилось через команду db['american-fast-food'].find():  
<img width="1032" alt="Screen Shot 2023-03-29 at 09 59 44" src="https://user-images.githubusercontent.com/71087982/228607235-beca55e7-f358-44f9-b27a-2ba97896afff.png">
  
  
## Запрос поиска find
