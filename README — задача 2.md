(HW-03) спринт 1
задание 2.
Для решения данной задачи мы можем создать класс, который будет взаимодействовать с базой данных и содержать методы для пополнения информации в таблицах базы данных.

python
import os
import psycopg2


class Database:
    def __init__(self):
        self.db_host = os.environ.get('fstr_db_host')
        self.db_port = os.environ.get('fstr_db_port')
        self.db_login = os.environ.get('fstr_db_login')
        self.db_pass = os.environ.get('fstr_db_pass')

    def connect(self):
        conn = psycopg2.connect(
            host=self.db_host,
            port=self.db_port,
            user=self.db_login,
            password=self.db_pass
        )
        return conn

    def insert_data(self, table, data):
        conn = self.connect()
        cur = conn.cursor()

        # Добавляем новую строку в таблицу с перевалами
        if table == "pervals":
            # Устанавливаем значение поля status равным "new"
            data["status"] = "new"

        # Создаем SQL-запрос для вставки данных
        columns = ', '.join(data.keys())
        values = tuple(data.values())
        placeholders = ', '.join(['%s'] * len(data))
        sql = f"INSERT INTO {table} ({columns}) VALUES {placeholders}"

        # Выполняем SQL-запрос
        cur.execute(sql, values)
        conn.commit()

        cur.close()
        conn.close()


Объяснение кода:
- Класс Database содержит конструктор __init__, который получает значения пути к базе данных, порта, логина и пароля из переменных окружения.
- Метод connect используется для установления подключения к базе данных с помощью модуля psycopg2.
- Метод insert_data принимает имя таблицы и словарь данных. Если таблица - "pervals", то значение поля "status" в словаре данных устанавливается равным "new". Далее, создается SQL-запрос для вставки данных и выполняется с помощью методов execute и commit объекта cursor.
- После выполнения запроса, соединение с базой данных закрывается.

Теперь, чтобы использовать данный класс, ты можешь создать экземпляр класса Database и вызывать метод insert_data для вставки данных в таблицу:

python
db = Database()

data = {
    "name": "John",
    "age": 25,
    "email": "john@example.com"
}

db.insert_data("users", data)


В этом примере, мы создаем объект базы данных db и затем добавляем новую строку в таблицу пользователей с данными из словаря data.