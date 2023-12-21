(HW-03) спринт 2
задание 1.
Документация к REST API:
=======================
Задача:
-------
Добавить документацию к REST API проекта.

Что было реализовано:
--------------------
- Определены и реализованы эндпоинты для взаимодействия с приложением через HTTP протокол.
- Реализована логика обработки запросов и формирования ответов.
- Использованы стандартные методы HTTP для CRUD операций.
- Валидация и обработка ошибок.

Как работать с API:
------------------
1. Регистрация пользователя
   - Описание: Регистрация нового пользователя в системе.
   - HTTP метод: POST
   - Путь: /signup
   
   Тело запроса (JSON):
   

   {
     "username": "example_user",
     "password": "password123",
     "email": "user@example.com"
   }
   

   
   Ответ:
   

   {
     "message": "User successfully registered"
   }
   

   
2. Авторизация пользователя
   - Описание: Авторизация зарегистрированного пользователя.
   - HTTP метод: POST
   - Путь: /login
   
   Тело запроса (JSON):
   

   {
     "username": "example_user",
     "password": "password123"
   }
   

   
   Ответ:
   

   {
     "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
   }
   

   
3. Получение информации о текущем пользователе
   - Описание: Получение информации о текущем авторизованном пользователе.
   - HTTP метод: GET
   - Путь: /user
   
   Заголовок запроса:
   

   Authorization: Bearer JWT_TOKEN
   

   
   Ответ:
   

   {
     "username": "example_user",
     "email": "user@example.com"
   }
   

   
4. Создание нового объекта
   - Описание: Создание нового объекта в системе.
   - HTTP метод: POST
   - Путь: /objects
   
   Заголовок запроса:
   

   Authorization: Bearer JWT_TOKEN
   

   
   Тело запроса (JSON):
   

   {
     "name": "Object 1",
     "description": "This is object 1"
   }
   

   
   Ответ:
   

   {
     "message": "Object created successfully"
   }
   

   
5. Получение списка объектов
   - Описание: Получение списка всех объектов в системе.
   - HTTP метод: GET
   - Путь: /objects
   
   Заголовок запроса:
   

   Authorization: Bearer JWT_TOKEN
   

   
   Ответ:
   

   {
     "objects": [
       {
         "name": "Object 1",
         "description": "This is object 1"
       },
       {
         "name": "Object 2",
         "description": "This is object 2"
       }
     ]
   }
   

   
6. Обновление данных объекта
   - Описание: Обновление данных уже существующего объекта.
   - HTTP метод: PUT
   - Путь: /objects/ObjectID
   
   Заголовок запроса:
   

   Authorization: Bearer JWT_TOKEN
   

   
   Тело запроса (JSON):
   

   {
     "name": "Updated Object",
     "description": "This is the updated object"
   }
   

   
   Ответ:
   

   {
     "message": "Object updated successfully"
   }
   

   
7. Удаление объекта
   - Описание: Удаление объекта из системы.
   - HTTP метод: DELETE
   - Путь: /objects/ObjectID
   
   Заголовок запроса:
   

   Authorization: Bearer JWT_TOKEN
   

   
   Ответ:
   

   {
     "message": "Object deleted successfully"
   }
   


Примеры вызова API с хостинга:
-----------------------------
Примеры вызова API можно найти в файле [examples.md](https://example.com/examples.md). 

Swagger документация:
---------------------
Swagger документацию можно найти по адресу [https://example.com/swagger](https://example.com/swagger). 
