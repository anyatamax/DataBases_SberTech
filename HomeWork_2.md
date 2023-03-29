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
Дальше чуть посложнее, посмотрим в нескольких городах и чтобы категория была не Fast Food Restaurant db['american-fast-food'].find( { city: { $in: ["Brooklyn", "Detroit", "Newton" ] }, categories: { $ne: "Fast Food Restaurant" } } ):  
<img width="1339" alt="Screen Shot 2023-03-29 at 14 44 36" src="https://user-images.githubusercontent.com/71087982/228609785-e25c67d0-cb5f-4fb7-901e-88036a278f46.png">  
<img width="1103" alt="Screen Shot 2023-03-29 at 14 44 12" src="https://user-images.githubusercontent.com/71087982/228610189-79925ba3-5559-475d-8fd0-1249e27c64ad.png">  
Теперь применим регулярные выражения и посмотрим в этих городах фаст-фуд, в названии которого есть слово American db['american-fast-food'].find( { city: ( $in: [ "Brooklyn", "Detroit", "Newton"] }, categories: { $regex:
"American" } } )  
<img width="1332" alt="Screen Shot 2023-03-29 at 10 37 21" src="https://user-images.githubusercontent.com/71087982/228611347-c01fa186-ba2f-4a64-aab1-b430a956884f.png">  
## Обновление данных  
Попробуем заменить какое то поле объекта с айдишником AWEw nira4HuVbedNUaj:  
<img width="1029" alt="Screen Shot 2023-03-29 at 14 52 39" src="https://user-images.githubusercontent.com/71087982/228661093-6564e7b7-cb2f-41fb-9176-e8d7e687d640.png">  
Изменим категорию командой db['american-fast-food'].update( { id:"AWEw nira4HuVbedNUai"), { $set: { categories: "Ice Cream Shop" } } ):  
<img width="1040" alt="Screen Shot 2023-03-29 at 14 52 48" src="https://user-images.githubusercontent.com/71087982/228661323-ef9184c4-faee-429b-b4b5-2828bc74d866.png">  
А теперь вставим вообще новый объект вместо этого:  
<img width="1372" alt="Screen Shot 2023-03-29 at 14 57 08" src="https://user-images.githubusercontent.com/71087982/228661507-7c7a1f26-2dce-4045-96d4-ef5f4c18d455.png">  
Теперь поэкспериментируем с множественной заменой и поменяем Макдональдс на наше русское название)  
<img width="1238" alt="Screen Shot 2023-03-29 at 14 59 25" src="https://user-images.githubusercontent.com/71087982/228661721-87d3550d-a816-45be-8b92-3f08e8cd86df.png">  
Вот такой командой:  
<img width="814" alt="Screen Shot 2023-03-29 at 15 00 22" src="https://user-images.githubusercontent.com/71087982/228661780-1193dd5c-3825-4fa9-89a1-05fc0b595322.png">  
И получилось!  
<img width="1239" alt="Screen Shot 2023-03-29 at 15 00 53" src="https://user-images.githubusercontent.com/71087982/228661849-4e2c5d72-8676-4efe-abde-ee3e1fc89382.png">  
## Удаление данных  
Удалим объект с айдишником AWEw nira4HuVbedNUaj:  
<img width="980" alt="Screen Shot 2023-03-29 at 19 06 04" src="https://user-images.githubusercontent.com/71087982/228662295-7d152d13-b2c3-4810-887e-743afaf333a3.png">  
А теперь попробуем удалить поле из объекта. 
<img width="966" alt="Screen Shot 2023-03-29 at 19 09 31" src="https://user-images.githubusercontent.com/71087982/228662615-ccffa979-9d9c-4a4a-9270-c98a8fe7f624.png">




