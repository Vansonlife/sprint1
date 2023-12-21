(HW-03) спринт 1
задание 3.
Для выполнения данной задачи по созданию REST API с одним методом "submitdata", предлагается выполнить следующие шаги:

1. Создать базу данных для хранения информации о перевалах. Для улучшения структуры базы данных можно использовать следующие дополнительные поля в таблице:
   - id: уникальный идентификатор перевала
   - name: название перевала
   - elevation: высота перевала
   - difficulty: сложность перевала
   - length: длина маршрута до перевала
   - region: регион, в котором находится перевал

2. Создать класс или классы для работы с базой данных. В качестве примера можно использовать следующий код на языке Python, используя библиотеку Flask:

python
from flask import Flask, jsonify, request
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://username:password@localhost/db_name'  # заменить на свои настройки подключения к базе данных
db = SQLAlchemy(app)

class Pass(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(255))
    elevation = db.Column(db.Integer)
    difficulty = db.Column(db.String(100))
    length = db.Column(db.Float)
    region = db.Column(db.String(255))

    def __init__(self, name, elevation, difficulty, length, region):
        self.name = name
        self.elevation = elevation
        self.difficulty = difficulty
        self.length = length
        self.region = region

@app.route('/submitdata', methods=['POST'])
def submit_data():
    data = request.json
    name = data['name']
    elevation = data['elevation']
    difficulty = data['difficulty']
    length = data['length']
    region = data['region']
    
    new_pass = Pass(name, elevation, difficulty, length, region)
    db.session.add(new_pass)
    db.session.commit()
    
    return jsonify({'message': 'Data submitted successfully'})

if __name__ == '__main__':
    app.run()


3. При запуске приложения Flask будет доступен REST API метод "POST /submitdata", который ожидает отправку данных в формате JSON. Пример JSON-запроса выглядит следующим образом:

json
{
    "name": "Pass Name",
    "elevation": 1000,
    "difficulty": "Medium",
    "length": 5.2,
    "region": "Mountain"
}


4. API метод получает данные из JSON-запроса, создает новый объект перевала "new_pass" с помощью класса Pass и сохраняет его в базе данных.

5. После успешного сохранения данных, API метод возвращает JSON-ответ с сообщением об успешном сохранении данных.
