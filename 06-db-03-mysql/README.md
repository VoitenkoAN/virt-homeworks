# Домашнее задание к занятию "3. MySQL"

## Введение

Перед выполнением задания вы можете ознакомиться с 
[дополнительными материалами](https://github.com/netology-code/virt-homeworks/blob/virt-11/additional/README.md).

## Задача 1

Используя docker поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume.

sudo docker run --name mysql8 -v $HOME/docker/volumes/mysql/data/:/var/lib/mysql -v $HOME/docker/volumes/mysql//bckp:/tmp/backup -e MYSQL_ALLOW_EMPTY_PASSWORD=yes --rm -d mysql:8

Изучите [бэкап БД](https://github.com/netology-code/virt-homeworks/tree/virt-11/06-db-03-mysql/test_data) и 
восстановитесь из него.

Перейдите в управляющую консоль `mysql` внутри контейнера.

Используя команду `\h` получите список управляющих команд.

Найдите команду для выдачи статуса БД и **приведите в ответе** из ее вывода версию сервера БД.

![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/a4371485-5e6d-488c-a49e-d64673262d4d)

Подключитесь к восстановленной БД и получите список таблиц из этой БД.

**Приведите в ответе** количество записей с `price` > 300.

![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/a39eeba5-c7ec-45c8-8894-4b48986adc7e)


В следующих заданиях мы будем продолжать работу с данным контейнером.

## Задача 2

Создайте пользователя test в БД c паролем test-pass, используя:
- плагин авторизации mysql_native_password
- срок истечения пароля - 180 дней 
- количество попыток авторизации - 3 
- максимальное количество запросов в час - 100
- аттрибуты пользователя:
    - Фамилия "Pretty"
    - Имя "James"
 
![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/b3ae1b61-3fda-488c-bcf8-5bd9eecb5fa7)

Предоставьте привелегии пользователю `test` на операции SELECT базы `test_db`. 

mysql> GRANT SELECT on test_db.* TO test;

Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES получите данные по пользователю `test` и 
**приведите в ответе к задаче**.
![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/41cc54c5-dc37-4cf8-bfc6-932580595d82)

## Задача 3

Установите профилирование `SET profiling = 1`.
Изучите вывод профилирования команд `SHOW PROFILES;`.

Исследуйте, какой `engine` используется в таблице БД `test_db` и **приведите в ответе**.
![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/9c039c9f-7f0b-40bf-878c-feaab41ab9b8)

Измените `engine` и **приведите время выполнения и запрос на изменения из профайлера в ответе**:
- на `MyISAM`
- на `InnoDB`
![image](https://github.com/VoitenkoAN/virt-homeworks/assets/110226611/f897c245-74be-40e4-89b6-19b034b61018)

## Задача 4 

Изучите файл `my.cnf` в директории /etc/mysql.

Измените его согласно ТЗ (движок InnoDB):
- Скорость IO важнее сохранности данных
- Нужна компрессия таблиц для экономии места на диске
- Размер буффера с незакомиченными транзакциями 1 Мб
- Буффер кеширования 30% от ОЗУ
- Размер файла логов операций 100 Мб

Приведите в ответе измененный файл `my.cnf`.
```
[mysqld]
pid-file        = /var/run/mysqld/mysqld.pid
socket          = /var/run/mysqld/mysqld.sock
datadir         = /var/lib/mysql
secure-file-priv= NULL

innodb_flush_log_at_trx_commit = 0 
innodb_file_per_table = 1
autocommit = 0
innodb_log_buffer_size	= 1M
key_buffer_size = 2448М
max_binlog_size	= 100M
```
---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
