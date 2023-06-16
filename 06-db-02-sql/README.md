# Домашнее задание к занятию 2. «SQL»


## Задача 1

Используя Docker, поднимите инстанс PostgreSQL (версию 12) c 2 volume, 
в который будут складываться данные БД и бэкапы.

### Ответ:
Приведите получившуюся команду или docker-compose-манифест.

[root@localhost 06-db-02-sql]$ docker pull postgres:12  
```
docker-compose.yaml
version: '3'
services:
 db:
   container_name: pg12
   image: postgres:12
   environment:
     POSTGRES_USER: lodyanyy
     POSTGRES_PASSWORD: 123456
     POSTGRES_DB: start_db
   ports:
     - "5432:5432"
   volumes:      
     - database_volume:/home/database/
     - backup_volume:/home/backup/

volumes:
 database_volume:
 backup_volume:

```
$ docker-compose up -d  
$ sudo docker exec -it pg12 bash


## Задача 2

В БД из задачи 1: 

- создайте пользователя test-admin-user и БД test_db;
- в БД test_db создайте таблицу orders и clients (спeцификация таблиц ниже);
- предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db;
- создайте пользователя test-simple-user;
- предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE этих таблиц БД test_db.

Таблица orders:

- id (serial primary key);
- наименование (string);
- цена (integer).

Таблица clients:

- id (serial primary key);
- фамилия (string);
- страна проживания (string, index);
- заказ (foreign key orders).

Приведите:

- итоговый список БД после выполнения пунктов выше;
- описание таблиц (describe);
- SQL-запрос для выдачи списка пользователей с правами над таблицами test_db;
- список пользователей с правами над таблицами test_db.

### Ответ:

user@localhost 06-db-02-sql]$ sudo docker exec -it pg12 bash  
root@187afc3b98f6:/# createdb test_db -U postgres  
root@187afc3b98f6:/# psql -d test_db -U postgres  
  psql (12.15 (Debian 12.15-1.pgdg110+1))  
  Type "help" for help.  

```
test_db=# \l
                                  List of databases
    Name     |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges   
-------------+----------+----------+------------+------------+-----------------------
 postgres    | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 postgres_db | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 template0   | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
             |          |          |            |            | postgres=CTc/postgres
 template1   | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
             |          |          |            |            | postgres=CTc/postgres
 test_db     | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
(5 rows)

test_db=# 
```
<img src="https://github.com/michail-77/virt-homeworks/blob/virt-11/06-db-02-sql/image/06-02_2.1.PNG" alt="06-02_2.1">
<img src="https://github.com/michail-77/virt-homeworks/blob/virt-11/06-db-02-sql/image/06-02_2.2.PNG" alt="06-02_2.2">
<img src="https://github.com/michail-77/virt-homeworks/blob/virt-11/06-db-02-sql/image/06-02_2.3.PNG" alt="06-02_2.3">

SELECT grantee, table_catalog, table_name, privilege_type  
FROM information_schema.table_privileges  
WHERE table_name IN ('orders','clients');  



## Задача 3

Используя SQL-синтаксис, наполните таблицы следующими тестовыми данными:

Таблица orders

|Наименование|цена|
|------------|----|
|Шоколад| 10 |
|Принтер| 3000 |
|Книга| 500 |
|Монитор| 7000|
|Гитара| 4000|

Таблица clients

|ФИО|Страна проживания|
|------------|----|
|Иванов Иван Иванович| USA |
|Петров Петр Петрович| Canada |
|Иоганн Себастьян Бах| Japan |
|Ронни Джеймс Дио| Russia|
|Ritchie Blackmore| Russia|

Используя SQL-синтаксис:
- вычислите количество записей для каждой таблицы.

Приведите в ответе:

    - запросы,
    - результаты их выполнения.

### Ответ:

test_db=# INSERT INTO Orders (наименование, цена)  
VALUES ('Шоколад', 10), ('Принтер', 3000), ('Книга', 500), ('Монитор', 7000), ('Гитара', 4000);  
INSERT 0 5  

test_db=# INSERT INTO clients (фамилия, "страна проживания")  
VALUES  
('Иванов Иван Иванович', 'USA'),  
('Петров Петр Петрович', 'Canada'),  
('Иоганн Себастьян Бах', 'Japan'),  
('Ронни Джеймс Дио', 'Russia'),  
('Ritchie Blackmore', 'Russia')  

<img src="https://github.com/michail-77/virt-homeworks/blob/virt-11/06-db-02-sql/image/06-02_3.1.PNG" alt="06-02_3.1">

## Задача 4

Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.

Используя foreign keys, свяжите записи из таблиц, согласно таблице:

|ФИО|Заказ|
|------------|----|
|Иванов Иван Иванович| Книга |
|Петров Петр Петрович| Монитор |
|Иоганн Себастьян Бах| Гитара |

Приведите SQL-запросы для выполнения этих операций.

Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод этого запроса.
 
Подсказка: используйте директиву `UPDATE`.

### Ответ:

<img src="https://github.com/michail-77/virt-homeworks/blob/virt-11/06-db-02-sql/image/06-02_4.1.PNG" alt="06-02_4.1">


## Задача 5

Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 
(используя директиву EXPLAIN).

Приведите получившийся результат и объясните, что значат полученные значения.

```
test_db=# EXPLAIN SELECT * from clients where "заказ" is not null;
                        QUERY PLAN                         
-----------------------------------------------------------
 Seq Scan on clients  (cost=0.00..18.10 rows=806 width=72)
   Filter: ("заказ" IS NOT NULL)
(2 rows)

test_db=# 
```
Seq Scan - последовательное сканирование  
0.00 — затраты на получение первой строки
18.10 - затраты на получение всех строк  
rows – отображает число записей, обработанных для получения выходных данных  
width — средний размер одной строки в байтах.  

## Задача 6

Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. задачу 1).

Остановите контейнер с PostgreSQL, но не удаляйте volumes.

Поднимите новый пустой контейнер с PostgreSQL.

Восстановите БД test_db в новом контейнере.

Приведите список операций, который вы применяли для бэкапа данных и восстановления. 

### Ответ:

root@187afc3b98f6:/# pg_dump -U postgres -W test_db > /home/backup/test_db.back
Password: 
root@187afc3b98f6:/# ls -la /home/backup/
total 8
drwxr-xr-x. 2 root root   26 Jun 15 17:45 .
drwxr-xr-x. 1 root root   20 Jun  8 15:02 ..
-rw-r--r--. 1 root root 4292 Jun 15 17:45 test_db.back
[root@localhost 06-db-02-sql]# docker stop pg12
[root@localhost 06-db-02-sql]# docker run --name pg12_recovery -e POSTGRES_PASSWORD=12345 -v 06-db-02-sql_backup_postgres:/data -d postgres:12
ea669c23e5d5c459167f4dcc3e799bcdeae2a558b4ed42c99caea4d6be8fc4d5

docker exec -it pg12_recovery bash

root@ea669c23e5d5:/# cd /data
root@ea669c23e5d5:/data# ls -la
total 8
drwxr-xr-x. 2 root root   26 Jun 15 17:45 .
drwxr-xr-x. 1 root root   29 Jun 16 20:38 ..
-rw-r--r--. 1 root root 4292 Jun 15 17:45 test_db.back

createdb test_db -U postgres
root@ea669c23e5d5:/data# psql -U postgres test_db < /data/test_db.back
root@ea669c23e5d5:/data#  psql -d test_db -U postgres
psql (12.15 (Debian 12.15-1.pgdg110+1))
Type "help" for help.

test_db=# SELECT * from clients where "заказ" is not null;
```
 id |       фамилия        | страна проживания | заказ 
----+----------------------+-------------------+-------
  1 | Иванов Иван Иванович | USA               |     3
  2 | Петров Петр Петрович | Canada            |     4
  3 | Иоганн Себастьян Бах | Japan             |     5
(3 rows)
```
test_db=# 
