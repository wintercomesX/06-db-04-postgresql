# Домашнее задание к занятию "`PostgreSQL`" - `Кандала Кирилл`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1


1. `Решение задач приведено в поле для вставки кода`

```

# Listing databases
\l

# Connecting to the database
\c test_database

# Listing tables
\dt

# Displaying descriptions of table contents
\d orders

# Exiting psql
\q

```

---

### Задание 2

1. `Решение задач приведено в поле для вставки кода`

```

#Creating the test_database database:

CREATE DATABASE test_database;

#Restoring a backup to test_database:

pg_restore -d test_database /etc/backup.dump

#Connect to the test_database database:

psql -U postgres -d test_database

#ANALYZE operation to collect statistics on the orders table:

ANALYZE orders;

#Finding the column with the largest average element size in bytes:

SELECT avg_width FROM pg_stats WHERE tablename = 'orders' ORDER BY avg_width DESC LIMIT 1;

#Result

 avg_width
------------
         10

```

---

### Задание 3

1. `Решение задач приведено в поле для вставки кода`
2. `Да, можно было изначально исключить ручное разбиение при проектировании таблицы orders, используя партиционирование. Партиционирование позволяет автоматически распределять данные по нескольким таблицам на основе определенного критерия, что упрощает управление данными и оптимизирует производительность.`


```

#SQL transaction for splitting the orders table into two tables orders_1 and orders_2:

BEGIN TRANSACTION;

-- Создаем таблицы orders_1 и orders_2
CREATE TABLE orders_1 AS SELECT * FROM orders WHERE price > 499;
CREATE TABLE orders_2 AS SELECT * FROM orders WHERE price <= 499;

-- Удаляем исходную таблицу orders
DROP TABLE orders;

-- Переименовываем таблицы orders_1 и orders_2 в orders
ALTER TABLE orders_1 RENAME TO orders;
ALTER TABLE orders_2 RENAME TO orders;

COMMIT;

```

### Задание 4

1. `Решение задач приведено в поле для вставки кода`
2. `Чтобы добавить уникальность значения столбца title для таблиц в базе данных test_database, необходимо сначала изменить структуру таблицы orders и добавить ограничение уникальности для столбца title. Затем можно создать новый бэкап базы данных, который будет включать это ограничение.`


```

#Creating a backup of the test_database database

pg_dump -Fc -U postgres -d test_database -t orders -h localhost -p 5432 -w -f backup.dump

```

