Источник данных OData Feed https://services.odata.org/V3/Northwind/Northwind.svc/
Загружаем таблицы:

Orders
Order_Details
ProductsУстанавливаем связи между таблицами
Order_Details - Orders
Order_Details - Products
Создаем календарь с полями:
Date (Дата)
Year (Год)
MonthName (Имя месяца)
MonthNumber (Номер месяца в году)
DayMonthNumber (Номер дня в году)

Устанавливаем связь между таблицами
DimDates[Date] - Orders[OrderDate]
Создаем иерархию с полями:
Year
MonthName
DayMonthNumber
Устанавливаем сортировку поля MonthName по MonthNumber
Создаем меру Amount, которая рассчитывает объем выручки, как сумма выражения 'Order_Details'[Quantity] * 'Order_Details'[UnitPrice] по всем соответствующим строкам Order_Details
Отображаем матрицу, у которой в строках отображаем иерархию дат:

И добавляем следующие столбцы:

Выручка за соответствующий период
Выручка за аналогичный период год назад
Выручка накопительно за текущий год

Решение задачи :

Calendar = ADDCOLUMNS(
    CALENDARAUTO(),
"Year",
YEAR([Date]),
"Month name",
FORMAT([Date],"MMMM"),
"MonthNumber",
MONTH([Date]),
"DayMonthNumber",
FORMAT([Date],"DD"))
1 . Создала календарь
2. Создала 3 меры
- Amount = SUMX(RELATEDTABLE('Order_Details'),'Order_Details'[Quantity]*'Order_Details'[UnitPrice])
- Amount total YTD = TOTALYTD('Order_Details'[Amount],'Calendar'[Date]) 
- Amount same period Last Year = CALCULATE([Amount],SAMEPERIODLASTYEAR('Calendar'[Date]))
- 3.Cделала визуализацию
