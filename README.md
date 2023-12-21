(HW-03) спринт 1
задание 1.
Для выполнения этой задачи мне нужно установить PostgreSQL, так как эта задача требует доступа к базе данных. После установки PostgreSQL я смогу продолжить работу.

Дополнительно, если есть возможность улучшить структуру базы данных, это принесет мне дополнительные баллы. Поэтому я также обратил внимание на текущую структуру базы данных и выявил следующие проблемы:

1. В таблице "pereval_added" есть поле "raw_data", которое содержит огромный JSON с информацией об объекте. Это делает поиск самого высокого перевала или всех перевалов, отправленных конкретным пользователем, сложным.

2. Мне представляется, что улучшение базы данных включает следующие шаги:

- Создание отдельной таблицы "users" для хранения данных о каждом пользователе. Предлагается использовать email в качестве первичного ключа, чтобы не было двух пользователей с одинаковым email. Также можно сделать поле "id" первичным ключом и добавить уникальное ограничение на поле "email" с помощью параметра "constraint unique".

- В таблице "pereval_added" нужно добавить следующие поля: "beautytitle", "title", "other_titles", "connect" и "add_time".

- Создание новой таблицы "coords" с полями "id", "latitude" (широта), "longitude" (долгота) и "height" (высота). В таблице "pereval_added" будет храниться только идентификатор координаты "coord_id".

- Поле "level", которое представляет уровень сложности перевала в разное время года, также неэффективно хранить в виде JSON. Лучше добавить в таблицу "pereval_added" четыре текстовых поля, отображающих уровень сложности для каждого сезона.

- В настоящее время названия изображений и их идентификаторы хранятся в поле "images" таблицы перевалов в виде JSON. Рекомендуется создать отдельную таблицу "pereval_images", чтобы установить связь между таблицей перевалов и изображениями. Поле "images" будет удалено из таблицы "pereval_added", и вместо него будет новая таблица, хранящая два поля: идентификатор перевала и идентификатор изображения.

Теперь, имея эту информацию, я могу приступить к выполнению задачи.

К спринту: 
Для решения данной задачи, нам необходимо создать базу данных, написать класс для работы с ней и реализовать метод submitdata для REST API.

По условию, метод submitdata принимает JSON в теле запроса, содержащий информацию о перевале. Для начала, определим структуру JSON:

json
{
  "beauty_title": "пер.",
  "title": "пхия",
  "other_titles": "триев",
  "connect": "",
  "add_time": "2021-09-22 13:18:13",
  "user": {
    "email": "qwerty@mail.ru",
    "fam": "пупкин",
    "name": "василий",
    "otc": "иванович",
    "phone": "+7 555 55 55"
  },
  "coords": {
    "latitude": "45.3842",
    "longitude": "7.1525",
    "height": "1200"
  },
  "level": {
    "winter": "",
    "summer": "1а",
    "autumn": "1а",
    "spring": ""
  },
  "images": [
    { "data": "<картинка1>", "title": "седловина" },
    { "data": "<картинка>", "title": "подъём" }
  ]
}


Теперь приступим к реализации. Для создания базы данных можно использовать любую подходящую систему управления базами данных (например, MySQL, PostgreSQL и другие). В данном случае, я буду использовать пример с SQLite.

1. Создаем базу данных и таблицу в ней:

python
import sqlite3

# Подключаемся к БД
conn = sqlite3.connect('database.db')
cursor = conn.cursor()

# Создаем таблицу
cursor.execute('''
    CREATE TABLE IF NOT EXISTS peaks (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        beauty_title TEXT,
        title TEXT,
        other_titles TEXT,
        connect TEXT,
        add_time DATETIME,
        email TEXT,
        fam TEXT,
        name TEXT,
        otc TEXT,
        phone TEXT,
        latitude TEXT,
        longitude TEXT,
        height INTEGER,
        winter TEXT,
        summer TEXT,
        autumn TEXT,
        spring TEXT
    )
''')

# Сохраняем изменения и закрываем соединение
conn.commit()
conn.close()


2. Теперь создадим класс, который будет отвечать за работу с базой данных:

python
import sqlite3

class Database:
    def __init__(self, db_file):
        self.db_file = db_file
    
    def submit_data(self, data):
        # Извлекаем нужные данные из JSON
        beauty_title = data.get('beauty_title')
        title = data.get('title')
        other_titles = data.get('other_titles')
        connect = data.get('connect')
        add_time = data.get('add_time')
        user = data.get('user')
        email = user.get('email')
        fam = user.get('fam')
        name = user.get('name')
        otc = user.get('otc')
        phone = user.get('phone')
        coords = data.get('coords')
        latitude = coords.get('latitude')
        longitude = coords.get('longitude')
        height = coords.get('height')
        level = data.get('level')
        winter = level.get('winter')
        summer = level.get('summer')
        autumn = level.get('autumn')
        spring = level.get('spring')
        images = data.get('images')
        
        # Подключаемся к БД
        conn = sqlite3.connect(self.db_file)
        cursor = conn.cursor()
        
        # Вставляем данные в таблицу
        cursor.execute('''
            INSERT INTO peaks (
                beauty_title, title, other_titles, connect, add_time, email,
                fam, name, otc, phone, latitude, longitude, height,
                winter, summer, autumn, spring
            )
                VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?)
        ''', (
            beauty_title, title, other_titles, connect, add_time, email,
            fam, name, otc, phone, latitude, longitude, height,
            winter, summer, autumn, spring
        ))
        
        # Получаем id вставленной записи
        peak_id = cursor.lastrowid
        
        # Сохраняем изменения и закрываем соединение
        conn.commit()
        conn.close()
        
        return {"status": 200, "message": "отправлено успешно", "id": peak_id}


3. Теперь создаем REST API с единственным методом submitdata:

python
from flask import Flask, request
import json

app = Flask(__name__)
db = Database('database.db')

@app.route('/submitdata', methods=['POST'])
def submit_data():
    try:
        data = json.loads(request.data)
        result = db.submit_data(data)
        return json.dumps(result)
    except:
        return json.dumps({"status": 500, "message": "ошибка при выполнении операции"})

if __name__ == '__main__':
    app.run()