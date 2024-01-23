### Задача 1
Используя Docker, поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume.  
`docker-compose -f docker-compose.yml up -d`  
[docker-compose.yml](https://github.com/Svalker1989/MySQL/blob/main/docker-compose.yml)  
Изучите бэкап БД и восстановитесь из него.  
`mysql -p test_db < /tmp/mysql/backup/test_dump.sql`  
Перейдите в управляющую консоль mysql внутри контейнера.  
Используя команду \h, получите список управляющих команд.  
Найдите команду для выдачи статуса БД и приведите в ответе из её вывода версию сервера БД.  
`status`  
![](https://github.com/Svalker1989/MySQL/blob/main/Z1_1.PNG)  
Подключитесь к восстановленной БД и получите список таблиц из этой БД.  
`show tables;`  
![](https://github.com/Svalker1989/MySQL/blob/main/Z1_2.PNG)  
Приведите в ответе количество записей с price > 300.  
`select count(*) from orders where price > 300;`  
![](https://github.com/Svalker1989/MySQL/blob/main/Z1_3.PNG)  
В следующих заданиях мы будем продолжать работу с этим контейнером.  
  
### Задача 2
Создайте пользователя test в БД c паролем test-pass, используя:  
плагин авторизации mysql_native_password  
срок истечения пароля — 180 дней  
количество попыток авторизации — 3  
максимальное количество запросов в час — 100  
аттрибуты пользователя:  
Фамилия "Pretty"  
Имя "James".  
Предоставьте привелегии пользователю test на операции SELECT базы test_db.  
Ответ:  
```SQL
CREATE USER 'test'@'localhost'
  IDENTIFIED WITH mysql_native_password BY 'test-pass'
  WITH MAX_QUERIES_PER_HOUR 100
  PASSWORD EXPIRE INTERVAL 180 DAY
  FAILED_LOGIN_ATTEMPTS 3 
  ATTRIBUTE '{"Фамилия": "Pretty", "Имя ": "James"}';
```  
Выдаем права пользователю test на все таблицы БД test_db:   
`grant select on table test_db.* to 'test'@'localhost';`  
Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES, получите данные по пользователю test и приведите в ответе к задаче.  
Запрос:  
`select * from INFORMATION_SCHEMA.USER_ATTRIBUTES where user = test;`  
![](https://github.com/Svalker1989/MySQL/blob/main/Z2.PNG)  
### Задача 3
Установите профилирование SET profiling = 1. Изучите вывод профилирования команд SHOW PROFILES;.  
Исследуйте, какой engine используется в таблице БД test_db и приведите в ответе.  
`show table status;`  
![](https://github.com/Svalker1989/MySQL/blob/main/Z3_0.PNG)  
Измените engine и приведите время выполнения и запрос на изменения из профайлера в ответе:  
на MyISAM,  
`alter table orders engine=myisam;`  
![](https://github.com/Svalker1989/MySQL/blob/main/Z3_2(MyISAM).PNG)  
на InnoDB.  
`alter table orders engine=InnoDB;`  
![](https://github.com/Svalker1989/MySQL/blob/main/Z3_1(InnoDB).PNG)  
### Задача 4
Изучите файл my.cnf в директории /etc/mysql.  
Измените его согласно ТЗ (движок InnoDB):  
скорость IO важнее сохранности данных;  
нужна компрессия таблиц для экономии места на диске;  
размер буффера с незакомиченными транзакциями 1 Мб;  
буффер кеширования 30% от ОЗУ;  
размер файла логов операций 100 Мб.  
Приведите в ответе изменённый файл my.cnf.  
Описание внутри:  
[my.cnf](https://github.com/Svalker1989/MySQL/blob/main/my.cnf)
