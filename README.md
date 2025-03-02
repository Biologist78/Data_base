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

Какие данные хранятся в этих таблицах и тип данных:

1. ФИО - фамилия имя и отчество сотрудника          строковый (varchar), числовой (integer)
2. ОКЛАД - заработная плата сотрудника              числовой (decimal/numeric). Если СУБД PostgreSQL то MONEY
3. Должность - Занимаемая должность сотрудника      строковый (varchar), числовой (integer)
4. Подразделение - название отдела                  строковый (varchar), числовой (integer)
5. Тип - тип отдела                                 строковый (varchar), числовой (integer)
8. Дата - дата найма работника                      дата (DATE)
7. Адрес - местонахождение филиала                  строковый (varchar), числовой (integer)
8. Проект - проект, назначенный сотруднику          строковый (varchar), числовой (integer)

Сотрудники (
    идентификатор ФИО, первичный ключ, serial, integer
    оклад, DECIMAL (10, 2),
    идентификатор должность, первичный ключ, serial, integer
    идентификатор тип подразделения, первичный ключ, serial, integer
    идентификатор структурное подразделение, первичный ключ, serial, integer 
    дата найма, DATE,
    идентификатор адрес, первичный ключ, serial, integer 
)
ФИО (
    идентификатор ФИО, первичный ключ, serial, integer
    фамилия имя отчество varchar(160),
)
должность (
    идентификатор должности, первичный ключ, serial, integer 
    должность varchar(70),
)
тип подразделения (
    идентификатор, первичный ключ, serial, integer 
    тип подразделения varchar(50),
)
структурное подразделение (
    идентификатор, первичный ключ, serial, integer
    структурное подразделение varchar(70),
)
адрес (
    идентификатор адреса, первичный ключ, serial, integer 
    адрес varchar(70),
)
проект (
    идентификатор проекта первичный ключ, serial, integer
    проект varchar(70),
)

---

### Задание 2*

Перечислите, какие, на ваш взгляд, в этой денормализованной таблице встречаются функциональные зависимости и какие правила вывода нужно применить, чтобы нормализовать данные.

#### Ответ

Функциональные зависимости денормализованной таблицы:

Одному ФИО соответствуют одно неповторяющееся и полностью от него зависящее значение Оклада, Даты найма и Проекта. Поэтому, для нормализации данных, их можно вынести в отдельные таблицы и полученные ID внести в виде внешнего ключа (foreign key) в employees_id.