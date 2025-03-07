# Домашнее задание к занятию "2. SQL"

## Введение

Перед выполнением задания вы можете ознакомиться с 
[дополнительными материалами](https://github.com/netology-code/virt-homeworks/blob/virt-11/additional/README.md).

## Задача 1

Используя docker поднимите инстанс PostgreSQL (версию 12) c 2 volume, 
в который будут складываться данные БД и бэкапы.

Приведите получившуюся команду или docker-compose манифест.
```
sudo docker run --name pg_docker2 -e POSTGRES_PASSWORD=123 -p 5432:5432/tcp -v $HOME/docker/volumes/postgres/data:/var/lib/postgresql/data -v $HOME/docker/volumes/postgres/bckp:/var/lib/postgresql/bckp -d postgres:12
```

## Задача 2

В БД из задачи 1: 
- создайте пользователя test-admin-user и БД test_db
- в БД test_db создайте таблицу orders и clients (спeцификация таблиц ниже)
- предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db
- создайте пользователя test-simple-user  
- предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE данных таблиц БД test_db

Таблица orders:
- id (serial primary key)
- наименование (string)
- цена (integer)

Таблица clients:
- id (serial primary key)
- фамилия (string)
- страна проживания (string, index)
- заказ (foreign key orders)

Приведите:
- итоговый список БД после выполнения пунктов выше,

![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/1e57ae95-3d2e-4890-915c-36505c9286fb)

- описание таблиц (describe)

![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/ed69d26c-3ffc-4ed1-a0d9-71129ca607bf)

![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/b4408cbc-6111-4566-8bab-c621c67045e7)



- SQL-запрос для выдачи списка пользователей с правами над таблицами test_db]

![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/4c9693cc-12fc-4a4b-ab98-41ca529a421d)

![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/732a4222-02b0-4600-b2d9-aad26118dad7)



- список пользователей с правами над таблицами test_db

![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/ae0ed93c-18ca-46d4-881d-a9dcc57f4c7f)


## Задача 3

Используя SQL синтаксис - наполните таблицы следующими тестовыми данными:

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

Используя SQL синтаксис:
- вычислите количество записей для каждой таблицы 
- приведите в ответе:
    - запросы 
    - результаты их выполнения.
 ```
SELECT COUNT(*) FROM orders;
SELECT COUNT(*) FROM clients;
```
 ![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/6e5ff66e-e54a-4f21-a2bd-227bc4054a0d)

  ![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/a1d6af28-e35b-4110-b85c-8128dbca4ade)


## Задача 4

Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.

Используя foreign keys свяжите записи из таблиц, согласно таблице:

|ФИО|Заказ|
|------------|----|
|Иванов Иван Иванович| Книга |
|Петров Петр Петрович| Монитор |
|Иоганн Себастьян Бах| Гитара |

Приведите SQL-запросы для выполнения данных операций.
```
UPDATE clients SET order_number=3 WHERE id=1;
UPDATE clients SET order_number=4 WHERE id=2;
UPDATE clients SET order_number=5 WHERE id=3;
```
Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод данного запроса.
```
SELECT * FROM clients WHERE order_number IS NOT NULL
```
 ![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/2f10e05c-2133-426b-92b2-f8ea89fb3120)

Подсказк - используйте директиву `UPDATE`.

## Задача 5

Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 
(используя директиву EXPLAIN).

![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/2c0ca39a-8531-4000-8c6e-1b48f8310e0f)


```
EXPLAIN - позволяет нам дать служебную информацию о запросе к БД, в том числе время на выполнение запроса, что при оптимизации работы БД является очень полезной информацией.
```

Приведите получившийся результат и объясните что значат полученные значения.

## Задача 6

Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. Задачу 1).
```
pg_dumpall -U postgres >/var/lib/postgresql/bckp/bckp_"`date +"%d-%m-%Y"`"
```
Остановите контейнер с PostgreSQL (но не удаляйте volumes).

```
sudo docker stop 72f9239fbaed
```

Поднимите новый пустой контейнер с PostgreSQL.

sudo docker run --name pg_docker3 -e POSTGRES_PASSWORD=123 -p 5432:5432/tcp -v $HOME/docker/volumes/postgres/data:/var/lib/postgresql/data -v $HOME/docker/volumes/postgres/bckp:/var/lib/postgresql/bckp -d postgres:12

Восстановите БД test_db в новом контейнере.

sudo docker exec -it 6cd93e85c080  /bin/bash
psql -U postgres -f /var/lib/postgresql/bckp/bckp_23-06-2023

Приведите список операций, который вы применяли для бэкапа данных и восстановления. 

---

### Как cдавать задание

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
