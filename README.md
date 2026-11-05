# Практическая работа №1: Интерактивная работа с MySQL в Docker

## 🎯 Цель работы
Запустить изолированный контейнер MySQL, освоить основные операции с базами данных (CRUD), настроить логирование в файл и создать резервную копию (дамп) базы данных.

## ⚙️ Выполненные шаги
1. Настроен файл конфигурации `my.cnf` для поддержки кириллицы (`utf8mb4`) и логирования в файл (`log_output = FILE`, `general_log = 1`).
2. Запущен Docker-контейнер на базе официального LTS-образа `mysql:8.0`.
3. Создана база данных `my_database` и таблица `users`.
4. Добавлены тестовые данные (3 пользователя), проверена работа с кириллицей.
5. Создан дамп базы данных `dump_clean.sql` с помощью утилиты `mysqldump` с флагами `--single-transaction` и `--set-gtid-purged=OFF`.
6. Извлечен файл лога `general.log` из контейнера.

## 📁 Описание файлов
* **`my.cnf`** — файл конфигурации MySQL (кодировки и настройки логирования).
* **`dump_clean.sql`** — дамп базы данных `my_database` (содержит CREATE TABLE и INSERT). Может быть использован для восстановления.
* **`general.log`** — лог-файл всех выполненных SQL-запросов.
* **`README.md`** — данный файл с описанием проделанной работы.

## 🚀 Как развернуть проект (Восстановление БД)
1. Запустите контейнер:
   `docker run --name my-mysql -e MYSQL_ROOT_PASSWORD=123 -v $(pwd)/my.cnf:/etc/mysql/conf.d/my.cnf -d mysql:8.0`
2. Создайте пустую базу данных:
   `docker exec -it my-mysql mysql -uroot -p123 -e "CREATE DATABASE my_database CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"`
3. Импортируйте данные из дампа:
   `docker exec -i my-mysql mysql -uroot -p123 my_database < dump_clean.sql`
