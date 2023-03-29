# MongoDB  
Для начала необходимо скачать Docker и Mongo Compass.  
Далее в папке, в которой работает, создем файл docker-compose.yml:  
<img width="519" alt="Screen Shot 2023-03-29 at 07 07 00" src="https://user-images.githubusercontent.com/71087982/228604642-83264cae-fe1a-4f1a-a7d1-767c9aeb4297.png">  
Затем запустем докер через команду:  
<img width="582" alt="Screen Shot 2023-03-29 at 07 06 46" src="https://user-images.githubusercontent.com/71087982/228605264-596fe46e-a4a2-4554-9345-a34a8dbd4340.png">  
Проверем, что все работает, в докере появится наш образ:  
<img width="1267" alt="Screen Shot 2023-03-29 at 07 06 15" src="https://user-images.githubusercontent.com/71087982/228605519-a2ff6e63-090e-4f88-9900-ef5bed0ce656.png">
В Mongo Compose так же получится подключиться по локальному хосту 27017:27017:  
<img width="1415" alt="Screen Shot 2023-03-29 at 07 06 23" src="https://user-images.githubusercontent.com/71087982/228605784-4519448f-996b-470b-8e38-8574cfa1e2d0.png">
Все заработало, значит можно создавать базу данных!  Создаем через: use fast food и db.createCollection('american-fast-food'). 
<img width="353" alt="Screen Shot 2023-03-29 at 09 54 08" src="https://user-images.githubusercontent.com/71087982/228606251-38b96887-bf4f-4e57-aeb1-0d30c80b3eb6.png">  
Берем уже готовую базу данных ([с сайта](https://www.kaggle.com/datasets/thedevastator/fast-food-restaurants-in-the-united-states?resource=download)) и загрузжаем ее через интерфейс программы Mongo Compose:  
 <img width="592" alt="Screen Shot 2023-03-29 at 09 53 51" src="https://user-images.githubusercontent.com/71087982/228606822-cdaac8e8-8fc2-411b-b3af-a4a41f6b771b.png">
Проверем, что все загрузилось через команду db['american-fast-food'].find():  
<img width="1032" alt="Screen Shot 2023-03-29 at 09 59 44" src="https://user-images.githubusercontent.com/71087982/228607235-beca55e7-f358-44f9-b27a-2ba97896afff.png">
  
  
## Запрос поиска find  
Узнаем есть ли фаст-фуд еда в Brooklyn: db['american-fast-food'].find( { city: "Brooklyn"} ): 
<img width="1032" alt="Screen Shot 2023-03-29 at 10 08 17" src="https://user-images.githubusercontent.com/71087982/228608189-d05f5948-32d0-4ab2-ad14-b3269f26cee1.png">
<img width="525" alt="Screen Shot 2023-03-29 at 10 08 26" src="https://user-images.githubusercontent.com/71087982/228608219-caeb0a30-4bf5-43ae-95e4-a4d1df55f446.png">  
А категории Fast Food Restaurants?  db['american-fast-food'].find( { city: "Brooklyn", categories:"Fast Food Restaurants"} ).  Уже поменьше)  
<img width="1036" alt="Screen Shot 2023-03-29 at 10 10 11" src="https://user-images.githubusercontent.com/71087982/228608627-c3b3a980-9973-45b1-af4e-f7a953168dc0.png">. 
<img width="794" alt="Screen Shot 2023-03-29 at 10 10 19" src="https://user-images.githubusercontent.com/71087982/228608652-d6931bec-aec4-438c-9bc5-d3ae02c78d84.png">

