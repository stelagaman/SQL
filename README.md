# SQL
![Image](https://github.com/user-attachments/assets/eba283bc-a24d-40bc-915a-40ecfc6fb171)
# 1.Посчитай, сколько компаний закрылось.

select count(status) from company
where status = 'closed';

# 2.Отобрази количество привлечённых средств для новостных компаний США. Используй данные из таблицы company. Отсортируй таблицу по убыванию значений в поле funding_total.

select funding_total from company
where category_code = 'news' and country_code = 'USA'
order by funding_total desc;

# 3.Отобрази имя, фамилию и названия аккаунтов людей в поле network_username, которые начинаются на 'Silver'.

            select first_name,
                        last_name,
                        network_username from people
            where network_username like 'Silver%';

# 4.Выведи на экран всю информацию о людях, у которых названия аккаунтов в поле network_username содержат подстроку 'money', а фамилия начинается на 'K'.

            select  * from people
            where (network_username like '%money%' and last_name like 'K%');

# 5.Для каждой страны отобрази общую сумму привлечённых инвестиций, которые получили компании, зарегистрированные в этой стране. Страну, в которой зарегистрирована компания, можно определить по коду страны. Отсортируй данные по убыванию суммы.

    select country_code,
            sum (funding_total) 
            from company group by country_code ORDER BY SUM(funding_total) DESC;

# 6.Отобрази имя и фамилию всех сотрудников стартапов. Добавь поле с названием учебного заведения, которое окончил сотрудник, если эта информация известна.

            select 
                p.first_name, 
                p.last_name,
                e.instituition 
             from people as p
             left join education as e on p.id=e.person_id;

# 7.Найди общую сумму сделок по покупке одних компаний другими в долларах. Отбери сделки, которые осуществлялись только за наличные с 2011 по 2013 год включительно.

            select sum(price_amount) from acquisition
            WHERE acquired_at BETWEEN '2011-01-01' AND '2013-12-31'
            and term_code = 'cash';

# 8.Выясни, в каких странах находятся фонды, которые чаще всего инвестируют в стартапы. Для каждой страны посчитай минимальное, максимальное и среднее число компаний, в которые инвестировали фонды этой страны, основанные с 2010 по 2012 год включительно. Исключи страны с фондами, у которых минимальное число компаний, получивших инвестиции, равно нулю. Выгрузи десять самых активных стран-инвесторов: отсортируй таблицу по среднему количеству компаний от большего к меньшему. Затем добавь сортировку по коду страны в лексикографическом порядке.

            select country_code,
                    min(invested_companies),
                    max(invested_companies), 
                    avg(invested_companies)
            from fund
            where founded_at BETWEEN '2010-01-01' AND '2012-12-31'
            group by country_code
            having min(invested_companies) > 0
            order by avg(invested_companies) desc, country_code 
            limit 10;
