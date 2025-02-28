# Домашнее задание к занятию "Базы данных" - Сенотов А.С.

### Задание 1

Опишите не менее семи таблиц, из которых состоит база данных:

* какие данные хранятся в этих таблицах;
* какой тип данных у столбцов в этих таблицах, если данные хранятся в PostgreSQL.
Приведите решение к следующему виду:

Сотрудники (

* идентификатор, первичный ключ, serial,
* фамилия varchar(50),
* ...
* идентификатор структурного подразделения, внешний ключ, integer).

#### Ответ

В идеале, таблицу необходимо привести к 1 НФ, где каждая сложная запись в отдельной ячейке и для каждой повторяющейся записи имеется собственная таблица. Но, так как необходимо не менее 7 таблиц, не будем плодить сущности и разделим наиболее повторяющиеся.

1. Выделим адрес в отдельную таблицу. При увеличении филиалов, их тоже можно и нужно будет разделить на регион, город, адрес, как только будут появляться филиалы.

office (
    office_id, TINYINT, PRIMARY KEY, SERIAL, NOT NULL
    адрес varchar(70), NOT NULL
)

2. Выделим подразделение в отдельную таблицу

division (
    division_id, TINYINT, PRIMARY KEY, SERIAL, NOT NULL
    структурное подразделение varchar(70), NOT NULL
)

3. Выделим тип подразделения в отдельную таблицу

div_type (
    div_type_id, TINYINT, PRIMARY KEY, SERIAL
    тип подразделения varchar(50), UNIQUE, NOT NULL
)

4. Выделим должность в отдельную таблицу (выделяем строковые данные, хотя необходимо их привести к атомарному состоянию в отдельной таблице)

positions (
    position_id, TINYINT, PRIMARY KEY, SERIAL
    должность varchar(70), UNIQUE, NOT NULL
)

5. Выделим проекты в отдельную таблицу

project (
    project_id TINYINT, PRIMARY KEY, SERIAL
    проект varchar(70), UNIQUE, NOT NULL
)

6. Выделим ФИО в отдельную таблицу (выделяем строковые данные, хотя необходимо их привести к атомарному состоянию в отдельной таблице, так как при текучке, расширении и тд, могут возникать парные значения имен, отчеств и даже фамилий.)

employees (
   employees_id TINYINT, PRIMARY KEY, SERIAL
   ФИО varchar(160), NOT NULL
)

7. Итоговая таблица с учетом связей

personal (
    employees_id    TINYINT, PRIMARY KEY, SERIAL
    оклад           DECIMAL (10, 2), NOT NULL
    position_id     TINYINT, PRIMARY KEY, SERIAL
    div_type_id     TINYINT, PRIMARY KEY, SERIAL 
    division_id     TINYINT, PRIMARY KEY, SERIAL, NOT NULL
    дата найма      DATE, NOT NULL
    office_id       TINYINT, PRIMARY KEY, SERIAL, NOT NULL
)

Типы данных указаны на схемах таблиц. В целом, типы данных в СУБД совпадают, но в PostgreSQL для финансово-денежных единиц существует тип MONEY

---

### Задание 2*

Перечислите, какие, на ваш взгляд, в этой денормализованной таблице встречаются функциональные зависимости и какие правила вывода нужно применить, чтобы нормализовать данные.

#### Ответ

Функциональные зависимости денормализованной таблицы:

Одному ФИО соответствуют одно неповторяющееся и полностью от него зависящее значение Оклада, Даты найма и Проекта. Поэтому, для нормализации данных, их можно вынести в отдельные таблицы и полученные ID внести в виде внешнего ключа (foreign key) в employees_id.