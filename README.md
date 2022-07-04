Выполнение Домашнее задание 2
# HW_DB
```SQL
CREATE DATABASE имя_БД; -- создать БД
CREATE TABLE имя_таблицы; -- создает таблицу 

Пример:
CREATE TABLE TEST (
	First_name varchar(100),
	Citi       varchar(80),
	Data       date
);

DROP TABLE имя_таблицы; -- удаление таблицы 

SELECT * FROM имя_таблицы;  -- выдаст результат самой таблицы

SELECT * First_name FROM имя_таблицы WHERE First_name = 'Max';
-- where - какие строки должны быть выбраны

Запрос INSERT -- ДОБАВЛЯЕТ СТРОКИ В ТАБЛИЦУ
INSERT INTO users (first_name, last_name) VALUES (‘Петя’, ‘Иванов’);

Запрос UPDATE -- ИЗМЕНЯЕТ ЗНАЧЕНИЯ УКАЗАННЫХ ВЛ ВСЕХ СТРОКАХ
UPDATE client SET first_name = 'Иван' WHERE first_name = 'Петя';

ALTER TABLE customer RENAME COLUMN Amount_2 TO Amount;  -- Переименование столбца

ALTER TABLE products RENAME TO items;                   -- Переименование таблицы


ПРАКТИКА:
update client set first_name = 'АНТОН', last_name = 'АНТОНОВ' where id = 5;
update client set first_name = 'СЕМЕН', last_name = 'СЕМЕНОВ', email = 'Semen69@mail.ru' where id =15;
update client set phone = '911' where id = 62; -- изменяем телефон в строке по id
delete from client where id between 89 and 95; -- удаляем с 89 по 95
select from client order by id; -- фильтррует по id 

delete from client where id (1,2);  --удаляем

```
