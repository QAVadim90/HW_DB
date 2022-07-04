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
Provider          Varchar,      -- поставщик.
Supplier_address  Varchar,    -- адрес постащика.
Amount_1            int,         -- количество.
Price             real,
Payment_type      Varchar,     -- Тип оплаты.
Customer          Varchar,     -- Заказчик.
Customer_address  Varchar,     -- Адрес заказчика.
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

Тема ПРАКТИКИ: Создание таблиц, наполнение таблиц и установка внешних связей

Задание:

Все скрипты сохранить и опубликовать в GitHub или GitLab.
Создать таблицы client, product, basket, country.
Таблица клиент должна иметь следующие колонки: uuid, first_name, last_name, phone, email, address, create_at. confirmed, country_id, balance.

Таблица product должна иметь следующие колонки: id, name_product, description_product, amount, price, provider, address_provider, country_provider.

Таблица basket должна иметь следующие колонки: id, id_client, id_product.

Таблица country должна иметь следующие колонки: id, name, short_code.

Декомпозировать те таблицы которые необходимо.
Наполнить таблицы данными по 10 строк в каждой таблице, применяя внешние связи (FK). вторичный ключ
Вывести данные таблицы product используя JOIN и присоединить зависимые таблицы.

```SQL
create database baza;

create extension if not exists "uuid-ossp";
create table client
(
uuid               uuid primary key,
first_name          Varchar,
last_name           Varchar,      
phone              Varchar,    
email              Varchar,        
address            Varchar,
create_a           date,     -- дата создания.
confirmed          boolean,  --подверждено. 
country_id         int,          
balance            real 
);


insert into client (uuid, first_name, last_name, phone, email, address, create_a, confirmed, balance, country_id) values
(uuid_generate_v4(), 'Владимир', 'Логинов', '123456', 'pam@gmail.com', 'Логиновская 100 кв 10', '2000.02.02', 't', '100', '1'),
(uuid_generate_v4(), 'Вера', 'Кожевникова', '12345', 'pum@gmail.com', 'Кожевническая 10 кв 10', '1999.01.01', 'f', '1000', '2'),
(uuid_generate_v4(), 'Анастасия', 'Курьянова', '1234', 'pim@gmail.com', 'Курьяновская 1 кв 1', '1998.03.03', 't', '10000', '2'),
(uuid_generate_v4(), 'Иван', 'Троян', '123', 'pah@gmail.com', 'Вредноносное ПО 2 кв 2', '1997.04.04', 't', '30', '4'),
(uuid_generate_v4(), 'Вадим', 'Писанец', '124', 'bun@gmail.com', 'Лучезарных строителей 10 кв 120', '2020.01.07', 'f', '70000', '6'),
(uuid_generate_v4(), 'Таня', 'Косач', '125', 'cd@gmail.com', 'Преподобных 10 кв 120', '2020.08.07', 't', '35000', '8'),
(uuid_generate_v4(), 'Вика', 'Сеньор', '46621', 'voice@gmail.com', 'Милых клоунов 10 кв 120', '2020.07.08', 'f', '10000', '1'),
(uuid_generate_v4(), 'Вика', 'Серова', '388012', 'vlc@gmail.com', 'Верных мужей 10 кв 120', '2020.07.08', 't', '46009', '3'),
(uuid_generate_v4(), 'Владимир', 'Денисенко', '137665', 'fifa@gmail.com', 'Красивых жен 10 кв 120', '2020.07.08', 'f', '576612', '1'),
(uuid_generate_v4(), 'Влад', 'Турусов', '080701', 'top@gmail.com', 'Умных детей 10 кв 120', '2020.07.08', 'f', '35700', '5');


alter table client  add column  country int references country(id);



create sequence product_id;
create table product
(
id int not null default nextval('product_id') primary key,
name_product        Varchar,        --название продукта.
description_produc  Varchar,        --описание его
amount              int,            --количество
price               real,
provider            Varchar,
address_provider    Varchar,
country_provider    Varchar
);


create sequence provider_id;
create table provider           
(
id int not null default nextval('provider_id') primary key,
provider            Varchar,
address_provider    Varchar,
country_provider    Varchar
);
alter table product drop column provider, drop column address_provider, drop column country_provider;
alter table product add column provider_id int references provider(id);

insert into provider (provider, address_provider, country_provider) values
('ooo жги', 'Королевская 4', 'Россия'),
('ooo газуй', 'ЗаМир 3', 'Украина'),
('ooo вперед', 'Била Мюрея 8', 'Беларусь'),
('ooo поднажми', 'Куцигина 11', 'Индонезия'),
('ooo тапкувпол', 'Ломоносова 16', 'Австралия'),
('ooo эгеей', 'Грибоедова 22', 'Албания'),
('ooo догоняй', 'Стрит 11', 'Иран'),
('ooo дрифт', 'Улочка 21', 'Ирак'),
('ooo тормози', 'Праздник 25', 'Италия'),
('ooo бензин', 'Фестиваль 15', 'Бразилия');



insert into product (name_product, description_produc, amount, price, provider_id) values
('Щетка для обуви', 'щетка', '150', '150', '1'),
('Гуталин',  'смазка', '50', '50',  '1'),
('Диффузор', 'деталь', '26', '50', '3'),
('Iphone', 'телефон', '1000', '1000', '1'),
('Чайник', 'электро', '250', '250', '5'),
('Разделочная доска', 'дерево', '15', '25', '6'),
('Блюдо', 'керамическая посуда', '20', '90', '7'),
('Нож', 'кухонный нож', '25', '45', '1'),
('Вилка', 'столовый прибор', '20', '50', '1'),
('Ложка', 'столовый прибор', '15', '10', '1');

--ЗАДАНИЕ 5 ВЫВОДИМ ТАБЛИЦУ PRODUCT ИСПОЛЬЗУЯ JOIN И ПРИСОЕДИНЯЕМ ЗАВИСИМЫЕ ТАБЛИЦЫ
select product.name_product, product.description_produc, product.amount, product.price, product.provider_id, provider.address_provider 
from  product inner join provider on product.provider_id  = provider.id;


create sequence if not exists basket_id;   --СОЗДАЕМ ПОСЛЕДОВАТЕЛЬНОСТЬ КОГДА ЕЕ НЕТ
create table basket
(
id int not null default nextval('basket_id') primary key,
client_id uuid references client(uuid),
product_id             int
);


insert into basket (id, client_id, product_id ) values
(1,(select uuid from client where first_name = 'Владимир'), (SELECT id FROM product WHERE name_product = 'Щетка для обуви')),
(2,(select uuid from client where first_name = 'Вера'), (SELECT id FROM product WHERE name_product = 'Гуталин')),
(3,(select uuid from client where first_name = 'Анастасия'), (SELECT id FROM product WHERE name_product = 'Диффузор')),
(4,(select uuid from client where first_name = 'Иван'), (SELECT id FROM product WHERE name_product = 'Iphone')),
(5,(select uuid from client where first_name = 'Вадим'), (SELECT id FROM product WHERE name_product = 'Чайник')),
(6,(select uuid from client where first_name = 'Таня'), (SELECT id FROM product WHERE name_product = 'Разделочная доска')),
(7,(select uuid from client where first_name = 'Вика'), (SELECT id FROM product WHERE name_product = 'Блюдо')),
(8,(select uuid from client where first_name = 'Вика'), (SELECT id FROM product WHERE name_product = 'Нож')),
(9,(select uuid from client where first_name = 'Владимир'), (SELECT id FROM product WHERE name_product = 'Вилка')),
(10,(select uuid from client where first_name = 'Влад'), (SELECT id FROM product WHERE name_product = 'Ложка'));


alter table basket  add column  product int references product(id);

create sequence country_id;
create table country
(
id int not null default nextval('country_id') primary key,
name              Varchar,
short_cod         int
);

insert into country (name, short_cod) values
('Россия', '10'),
('Канада', '11'),
('Австрия', '12'),
('Германия', '13'),
('США', '14'),
('Сербия', '15'),
('Болгария', '16'),
('Латвия', '17'),
('Тунис', '18'),
('Египет', '19');

```
