# Робота із базою даних в PHP

# Зміст

# mysqli

**Розширення mysqli**, або як його ще називають поліпшене (improved) розширення MySQL, було розроблено, щоб дати можливість програмістам в повній мірі скористатися функціоналом MySQL-сервера версій 4.1.3 і вище. Розширення mysqli включається в поставку PHP версій 5 і вище.

Для ввімкнення розширення потрібно(модифікувати php.ini):
1. Встановити директорію розширень

```ini
; Directory in which the loadable extensions (modules) reside.
; http://php.net/extension-dir
; extension_dir = "./"
; On windows:
extension_dir = "C:/php/ext"
```

2. Розкоментувати потрібне розширення

```ini
extension=php_mysqli.dll
```

## З'єднання із базою даних

Для відкриття з'єднання із базою даних потрібно в конструктор mysqli передати параметри підключення:

```php
$mysqli = new mysqli("localhost", "user", "password", "database");
```

## Виконання запитів

За виконання запитів відповідають функції **mysqli_query()**, **mysqli_real_query()** і **mysqli_multi_query()**. Найчастіше застосовується функція **mysqli_query()**, так як вона виконує відразу два завдання: виконує запит і буферизует на клієнті результат цього запиту (якщо він є). Виклик **mysqli_query()** ідентичний послідовному викликом функцій **mysqli_real_query ()** і **mysqli_store_result()**.

## DDL

```php
<?php
$mysqli = new mysqli("localhost", "root", "", "test");
$mysqli->query("DROP TABLE IF EXISTS test");
$mysqli->query("CREATE TABLE test(id INT, name text)");
```

![](../resources/img/db/img-1.png)

## Виконання CRUD

### CREATE

```php
$mysqli->query("INSERT INTO test(id,name) VALUES (1,'alex'), (2, 'john');");
```

![](../resources/img/db/img-2.png)

### READ

```php
$res = $mysqli->query("SELECT * FROM test;");

while ($row = $res->fetch_assoc()) {
    echo " id = " . $row['id'] . " name = ". $row['name'] . "\n";
}
```

![](../resources/img/db/img-3.png)

### UPDATE

```cpp
$res = $mysqli->query("SELECT * FROM test;");

while ($row = $res->fetch_assoc()) {
    echo " id = " . $row['id'] . " name = ". $row['name'] . "<br>";
}

$mysqli->query("UPDATE test set name = 'alex new' where id = 1;");

$res = $mysqli->query("SELECT * FROM test;");

while ($row = $res->fetch_assoc()) {
    echo " id = " . $row['id'] . " name = ". $row['name'] . "<br>";
} 
```

![](../resources/img/db/img-4.png)

### DELETE

```php
$mysqli->query("DELETE FROM test where id = 1;");
```

![](../resources/img/db/img-5.png)

### Весь код проекту

```php
<?php
$mysqli = new mysqli("localhost", "root", "", "test");
$mysqli->query("DROP TABLE IF EXISTS test");
$mysqli->query("CREATE TABLE test(id INT, name text)");

$mysqli->query("INSERT INTO test(id,name) VALUES (1,'alex'), (2, 'john');");

$res = $mysqli->query("SELECT * FROM test;");

while ($row = $res->fetch_assoc()) {
    echo " id = " . $row['id'] . " name = ". $row['name'] . "<br>";
}

$mysqli->query("UPDATE test set name = 'alex new' where id = 1;");

$res = $mysqli->query("SELECT * FROM test;");

while ($row = $res->fetch_assoc()) {
    echo " id = " . $row['id'] . " name = ". $row['name'] . "<br>";
}

$mysqli->query("DELETE FROM test where id = 1;");

$res = $mysqli->query("SELECT * FROM test;");

while ($row = $res->fetch_assoc()) {
    echo " id = " . $row['id'] . " name = ". $row['name'] . "<br>";
}
```

## Обробка помилок

```php
<?php
$mysqli = new mysqli("localhost", "root", "", "test");

if ($mysqli->connect_errno) {
    echo "Connection Error: (" . $mysqli->connect_errno . ") " . $mysqli->connect_error;
}

if(!$mysqli->query("DROP TABLE IF EXISTS test") || !$mysqli->query("CREATE TABLE test(id INT, name text)")){
	echo "Error while creating shema: (" . $mysqli->errno . ") " . $mysqli->error;
}

if(!$mysqli->query("INSERT INTO test(id,name) VALUES (1,'alex'), (2, 'john');")){
	echo "Cant insert: (" . $mysqli->errno . ") " . $mysqli->error;
}


$res = $mysqli->query("SELECT * FROM test;");

while ($row = $res->fetch_assoc()) {
    echo " id = " . $row['id'] . " name = ". $row['name'] . "<br>";
}

$mysqli->query("UPDATE test set name = 'alex new' where id = 1;");

$res = $mysqli->query("SELECT * FROM test;");

while ($row = $res->fetch_assoc()) {
    echo " id = " . $row['id'] . " name = ". $row['name'] . "<br>";
}

$mysqli->query("DELETE FROM test where id = 1;");

$res = $mysqli->query("SELECT * FROM test;");

while ($row = $res->fetch_assoc()) {
    echo " id = " . $row['id'] . " name = ". $row['name'] . "<br>";
}
```

## Транзакції

Алгоритм, при роботі із транзакціями:
- Почніть транзакцію за допомогою команди mysqli_begin_transaction.
- Виконайтеодну або більше команд SQL, наприклад SELECT, INSERT, UPDATE або DELETE.
- Перевірте, чи немає помилки і все відповідає вашим вимогам.
- Якщо є якісь помилки, то виконайте команду ROLLBACK, інакше виконайте команду COMMIT.

Синтаксис:

```php
mysqli_begin_transaction ( mysqli $link [, int $flags = 0 [, string $name ]] ) : bool
```

Коректні прапори:

- MYSQLI_TRANS_START_READ_ONLY: Стартувати транзакцію як "START TRANSACTION READ ONLY". Потрібно MySQL 5.6 або вище.
- MYSQLI_TRANS_START_READ_WRITE: Стартувати транзакцію як "START TRANSACTION READ WRITE". Потрібно MySQL 5.6 або вище.
- MYSQLI_TRANS_START_WITH_CONSISTENT_SNAPSHOT: Стартувати транзакцію як "START TRANSACTION WITH CONSISTENT SNAPSHOT".

До використання транзакцій:
```php

```

Після використання транзакцій:
```php
```

# pdo

## Завантажити проект

# Encryp and store password in db

## Завантажити проект

# csrf token

# Домашня робота

## Варіанти

# Контрольні запитання