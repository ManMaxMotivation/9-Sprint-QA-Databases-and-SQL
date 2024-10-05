# Работа с базой данных PostgreSQL

## Задание 1

Посчитай, сколько компаний закрылось.

SQL-Запрос:
```psql
SELECT COUNT(status)
FROM company
WHERE status LIKE '%closed%'
```

## Задание 2

Отобрази количество привлечённых средств для новостных компаний США. Используй данные из таблицы company. Отсортируй таблицу по убыванию значений в поле funding_total.

SQL-Запрос:
```sql
SELECT funding_total
FROM company
WHERE country_code = 'USA'
    AND category_code = 'news'
ORDER BY funding_total DESC
```

## Задание 3

Отобрази имя, фамилию и названия аккаунтов людей в поле network_username, которые начинаются на 'Silver'.

SQL-Запрос:
```sql
SELECT first_name, last_name, network_username
FROM people
WHERE network_username LIKE 'Silver%'
```

## Задание 4

Выведи на экран всю информацию о людях, у которых названия аккаунтов в поле network_username содержат подстроку 'money', а фамилия начинается на 'K'.

SQL-Запрос:
```sql
SELECT *
FROM people
WHERE network_username LIKE '%money%'
    AND last_name LIKE 'K%'
```

## Задание 5

Для каждой страны отобрази общую сумму привлечённых инвестиций, которые получили компании, зарегистрированные в этой стране. Страну, в которой зарегистрирована компания, можно определить по коду страны. Отсортируй данные по убыванию суммы.

SQL-Запрос:
```sql
SELECT SUM(funding_total),
    country_code
FROM company
GROUP BY country_code
ORDER BY SUM(funding_total) DESC
```

## Задание 6

Отобрази имя и фамилию всех сотрудников стартапов. Добавь поле с названием учебного заведения, которое окончил сотрудник, если эта информация известна.

SQL-Запрос:
```sql
SELECT e.instituition, p.first_name, p.last_name
FROM people AS p
LEFT OUTER JOIN education AS e ON p.id = e.person_id
```

## Задание 7

Найди общую сумму сделок по покупке одних компаний другими в долларах. Отбери сделки, которые осуществлялись только за наличные с 2011 по 2013 год включительно.

SQL-Запрос:
```sql
SELECT SUM(price_amount)
FROM acquisition
WHERE term_code = 'cash'
    AND CAST(acquired_at AS DATE) BETWEEN '2011-01-01' AND '2013-12-31'
```

## Задание 8

Выясни, в каких странах находятся фонды, которые чаще всего инвестируют в стартапы. 
Для каждой страны посчитай минимальное, максимальное и среднее число компаний, в которые инвестировали фонды этой страны, основанные с 2010 по 2012 год включительно. Исключи страны с фондами, у которых минимальное число компаний, получивших инвестиции, равно нулю. 
Выгрузи десять самых активных стран-инвесторов: отсортируй таблицу по среднему количеству компаний от большего к меньшему. Затем добавь сортировку по коду страны в лексикографическом порядке.

SQL-Запрос:
```sql
SELECT country_code,
    MIN(invested_companies),
    MAX(invested_companies),
    AVG(invested_companies)
FROM fund
WHERE CAST(founded_at AS DATE) BETWEEN '2010-01-01' AND '2012-12-31'
GROUP BY country_code
HAVING MIN(invested_companies) > 0
ORDER BY AVG(invested_companies) DESC,
    country_code
LIMIT 10;
```
