# Ответы на тест по SQL
1. Вывести Кол-во записей в справочнике товаров, удовлетворяющих следующим условиям:название товара содержит "томат" (без учета регистра) и объем единицы товара больше 1-го литра.
> SELECT COUNT(\*) FROM ANP.T_SQLT_ART WHERE NAME CONTAINS "томат" AND VOL_TRANSP > 1

2. Вывести Кол-во записей в справочнике товаров, удовлетворяющих следующим условиям:наименование ГР21 – Йогурты и 10 единиц товара имеют объём не более 2х литров.
> SELECT COUNT(\*) FROM ANP.T_SQLT_ART WHERE ART_GRP_LVL_1_NAME="Йогурты" 
> AND COUNT(SELECT VOL_TRANSP FROM ANP.T_SQLT_ART WHERE ART_GRP_LVL_1_NAME="Йогурты" > 2) >= 10

3. Вывести Кол-во записей в справочнике товаров, удовлетворяющих следующим условиям:вес единицы товара более 5и кг либо объём единицы товара более 7и литров.
> SELECT COUNT(\*) FROM ANP.T_SQLT_ART WHERE WEIGHT > 5 OR VOL_TRANSP > 7

4. Вывести Кол-во записей в справочнике товаров, удовлетворяющих следующим условиям:вес единицы товара кратен 1 кг и находится в диапазоне от 1 до 3х кг и объем единицы товара в диапазоне от 3х до 5и литров.
> SELECT COUNT(\*) FROM ANP.T_SQLT_ART WHERE (WEIGHT %1 = 0 AND WEIGHT >= 1 AND WEIGHT <= 3) OR (VOL_TRANSP >= 3 AND VOL_TRANSP <= 5)

5. Вывести средний/минимальный/максимальный вес товара для каждой ГР21.Данные отсортировать по возрастанию полей ГР20-ГР21.Результаты округлить до 2х знаков после запятой.
> SELECT ART_GRP_LVL_0_NAME, ART_GRP_LVL_1_NAME, AVG(WEIGHT), MIN(WEIGHT), MAX(WEIGHT) ORDER BY ART_GRP_LVL_0_NAME, ART_GRP_LVL_1_NAME FROM ANP.T_SQLT_ART

6. Вывести список товаров из ГР22 - Сливки, которые поместятся в мешок объемом 1 литр.Для позиций из списка нужно рассчитать, сколько целых товаров поместится в этот мешок.Данные отсортировать по возрастанию поля Название товара.
> SELECT NAME, 1 / VOL_TRANSP ORDER BY NAME FROM ANP.T_SQLT_ART WHERE VOL_TRANSP < 1

7. Определить среднюю плотность(кг/л) товаров из справочника товаров.Результат округлить до 2х знаков после запятой.
> SELECT ROUND(WEIGHT/VOL_TRANSP, 2) FROM ANP.T_SQLT_ART

8. Выведите максимальный, средний и минимальный объём товаров из справочника товаров тремя разными строчками, отсортировав по убыванию.
> SELECT MAX(VOLUME), AVG(VOLUME), MIN(VOLUME) FROM ANP.T_SQLT_ART

9. Выведите 5 товаров, набравших наибольшую суммарную выручку за 2018 годДанные отсортировать по убыванию значения суммарной выручки
> SELECT TOP 5 NAME, ART_GRP_LVL_0_NAME, SALE FROM ANP.T_SQLT_ART, ANP.T_SQLT_SALES WHERE ANP.T_SQLT_ART.ART_ID = ANP.T_SQLT_SALES.ART_ID
