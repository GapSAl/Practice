# Practice
14/07/22
create database July_14;

create sequence country_id;
create table country(
id serial primary key, name varchar, short_code real);

insert into country (name, short_code) values 
('China', 001),
('Norway', 002),
('Sweden', 003),
('France', 004),
('USA', 005),
('Canada', 006),
('India', 007),
('Germany', 008),
('Turkey', 009),
('Brazil', 010);
select * from country;

create extension if not exists "uuid-ossp";


create table client1
(id uuid not null primary key,
first_name varchar,
last_name varchar,
phone real,
email varchar,
address varchar,
create_at date,
confirmed boolean,
country_id int references country(id), balance real);


insert into client1 values
(uuid_generate_v4(), 'play', 'football', 89213628285,'vapahi3442@leupus.com', 'г. Москва ул. Васильева д.5', '12.04.2018', 'Yes', 2, 34.4),
(uuid_generate_v4(), 'play', 'hockey', 9632587412,'vapahi342@leupus.com', 'г. Екатеринбург ул. НОвая д.34', '18.04.2020', 'Yes', 3, 74.3 ),
(uuid_generate_v4(), 'play', 'tennis', 9874563217,'vapahi42@leupus.com',  'г. Ростов ул. Большая д.47', '03.03.2019', 'No', 1, 37.1),
(uuid_generate_v4(), 'play', 'swim', 1234567891,'vapahi344@leupus.com', 'г. Тамбов ул. Демидова д.38', '29.04.2020', 'Yes', 6, 53.9),
(uuid_generate_v4(), 'play', 'fly', 0148523697,'vapahi34@leupus.com','г. Псков ул. Минская д.73', '04.05.2021', 'No', 7, 387.9),
(uuid_generate_v4(), 'play', 'equestrain_sport', 036985214785,'vapahi442@leupus.com',  'г. Ярославль ул. Набережная д.89', '19.06.2022', 'No', 5, 29.7),
(uuid_generate_v4(), 'play', 'shooting', 7418529630,'vapahi42@leupus.com', 'г. Тверь ул. Южная д.74', '17.09.2017', 'Yes', 4, 92.8),
(uuid_generate_v4(), 'play', 'biathlon', 3698521470,'vapahi2@leupus.com',  'г. Кострома ул. Белая д.19', '16.05.2019', 'Yes', 8, 48.9),
(uuid_generate_v4(), 'play', 'skiing', 3216549870,'vapahi@leupus.com', 'г. Архангельск ул. Святая д. 85', '11.03.2017', 'No', 10, 45.9),
(uuid_generate_v4(), 'play', 'figure_skating', 5478963214,'vapahi44@leupus.com', 'г. Нижный Новгород ул. Верхняя д.37', '04.03.2018', 'No', 9, 29.9);
select * from client1;

create sequence provider_id;

create table provider
(id int not null default nextval('provider_id') primary key,
provider varchar,
address_provider varchar,
country_provider varchar);

insert into provider (provider, address_provider, country_provider)
values
('SPORT PLUS','г. Нижный Новгород ул. Верхняя д.37','China'),
('SPORTANGELS','Красноярский край, г. Красноярск, Западная ул.','Norway'),
('MASTER SPORT S.A.','Москва, Сыромятническая Нижняя ул., д.11','Sweden'),
('FC SPORT INVESTMENTS LIMITED','Москва, Южнобутовская ул., д.109','France'),
('SPORTIVNY MIR LIMITED','Москва, Беговая ул., д.15','USA'),
('INTERNATIONAL MOBILE SPORTSBOOK COMPANY SL','Москва, ул. Дмитровка М., д.16 стр 6','Canada'),
('SPORT-ATLANTIKA','Москва, Хоромный туп., д.4 к.8','India'),
('VITEX-SPORT','Санкт-Петербург, Среднеохтинский пр-кт, д.5','Germany'),
('COMPANY "PRO KENNEX SPORT"','Москва, пер. Знаменский Б., д. 8/12 стр. 4','Turkey'),
('SPORT-S-TOURS','Москва, пер. Знаменский Б., д.8/12 к.4','Brazil');

create sequence product_id;

create table product(
id serial primary key,
name_product varchar, description_product text, amount real,
price money, provider_id int references provider(id));

insert into product (name_product, description_product,amount,price) values 
('Детская одежда','К выбору названия для магазина одежды нужно подходить',11,32500),
('Мужская одежда','как к серьёзной творческой работе',12,32500),
('Вязаные вещи','Подбирая правильное наименование',13,32500),
('Женская одежда','необходимо учитывать множество факторов',14,32500),
('Головные уборы','Нельзя забывать о принципах нейминга',15,32500),
('Платья','Удачное название магазина',16, 8520),
('Носки','является одним из факторов успешного продвижения бизнеса',17,31500),
('Спортивная одежда','Использование иностранных слов',18, 1250),
('Трикотаж','Использование географических признаков',19,33500),
('Футболки','Оксюморон — это сочетание противоположных по смыслу слов',20,35500);
select * from product;

create sequence basket_id;
create table basket(
id serial primary key,
client1_id uuid references client1(id), 
product_id int references product(id));
--select * from basket;

insert into basket (client1_id, product_id)
values
((select id from client1 where phone = '89213628285'), 1),
((select id from client1 where phone = '9632587412'), 2),
((select id from client1 where phone = '9874563217'), 3),
((select id from client1 where phone = '1234567891'), 4),
((select id from client1 where phone = '0148523697'), 5),
((select id from client1 where phone = '03698521451'), 6),
((select id from client1 where phone = '7418529630'), 7),
((select id from client1 where phone = '3698521470'), 8),
((select id from client1 where phone = '3216549870'), 9),
((select id from client1 where phone = '5478963214'), 10);

select * from basket;

select product.id, product.name_product, product.description_product , 
product.amount , product.price , product.provider_id as provider_id, provider.provider , provider.address_provider , 
provider.country_provider,
country.name , country.short_code 
from product
right join provider on product.id=provider.id
left join country on provider.country_provider=country.id;
