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
Домашнее задание 4

Задание: 
1.	Дана таблица с товарами, поставщиками и заказчиками необходимо привести её к 3 нормальной форме в виде таблиц:
2.	Перевести декомпозированную таблицу в код с данными и внешними ключами (код приложить в виде sql файла на GitHub или GitLab)
```SQL
create database HW_DZ_4;

create sequence products_id;
create table products
(
id int not null default nextval('products_id') primary key,
"table"           uuid,
Name              Varchar,
Provider          Varchar,      - поставщик.
Supplier_address  Varchar,    - адрес постащика.
Amount_1            int,         - количество.
Price             real,
Payment_type      Varchar,     - Тип оплаты.
Customer          Varchar,     - Заказчик.
Customer_address  Varchar,     - Адрес заказчика.
Amount_2             int 
);



create sequence Provider_id;
create table Provider           - ПОСТАВЩИК
(
id int not null default nextval('Provider_id') primary key,
Provider          Varchar,
Supplier_address  Varchar
);
alter table products drop column Provider, drop column Supplier_address;
alter table products add column Provider_id int references Provider(id);

insert into Provider (Provider, Supplier_address) values
('ООО Богатеи', 'г. Орел, ул Разина, д. 5'),
('ООО Нищеброды', 'г. Изюм, ул. Кирова, д. 12'),
('ООО Добряки', 'г. Воронеж, ул. Киевская, д. 2');

create sequence Customer_id;
create table Customer           -- ЗАКАЗЧИК
(
id int not null default nextval('Customer_id') primary key,
Customer          Varchar,     
Customer_address  Varchar,     
Amount_2             int 
);
alter table products  drop column Customer, drop column Customer_address, drop Amount_2;
alter table products  add column Customer_id int references Customer(id);

ALTER TABLE customer RENAME COLUMN Amount_2 TO Amount;   -- Заменяем атрибут в customer или можно написать через as

insert into Customer (Customer, Customer_address, Amount) values
('ООО Отмолим ваши деньги', 'г.Воронеж ул пр. Труда 22', '150'),
('ООО Отмолим ваши деньги', 'г.Воронеж ул пр. Труда 22', '40'),
('ООО Отмолим ваши деньги', 'г.Воронеж ул пр. Труда 22', '40'),
('ООО Отмолим ваши деньги', 'г.Воронеж ул пр. Труда 22', '3'),
('ООО Отмолим ваши деньги', 'г.Воронеж ул пр. Труда 22', '260');

ALTER TABLE products  RENAME COLUMN Amount_1 TO Amount;   -- Заменяем атрибут в products


create extension if not exists "uuid-ossp"; --создаем генератор id

insert into products ("table", Name, amount, price, payment_type, Provider_id, Customer_id)
values 
(uuid_generate_v4(), 'Зубная паста', '100', '25', 'Наложенный пдатеж', '1', '1'),
(uuid_generate_v4(), 'Зубная нить',  '34', '300', 'Безналичный платеж',  '1', '2'),
(uuid_generate_v4(), 'Ручки шариковые', '55', '12', 'Наличный платеж', '1', '3'),
(uuid_generate_v4(), 'Вода минеральная', '2', '150', 'Наличный платеж', '2', '4'),
(uuid_generate_v4(), 'Вода минеральная', '350', '500', 'Безналичный платеж', '2', '5');

```
По факту, вполне можно и тип оплаты вынести как отдельную таблицу.
