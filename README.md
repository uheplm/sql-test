# Ответы на тест по SQL
## Тест был исправлен с помощью человека, разбирающегося в SQL. По голосовой связи мне были объяснены все ошибки которые я допустил в тесте. Решение написано мной с пояснительными комментариями.

1. Вывести Кол-во записей в справочнике товаров, удовлетворяющих следующим условиям:название товара содержит "томат" (без учета регистра) и объем единицы товара больше 1-го литра.
> SELECT COUNT(\*) FROM ANP.T_SQLT_ART WHERE NAME CONTAINS "томат" AND VOL_TRANSP > 1

2. Вывести Кол-во записей в справочнике товаров, удовлетворяющих следующим условиям:наименование ГР21 – Йогурты и 10 единиц товара имеют объём не более 2х литров.
> SELECT COUNT(*) FROM ANP.T_SQLT_ART WHERE ART_GRP_LVL_1_NAME="Йогурты" and VOL_TRANSP * 10 < 2

> Комментарий: Неправильно понял условие про 10 едениц товара. В итоге написал полную чепуху :)


3. Вывести Кол-во записей в справочнике товаров, удовлетворяющих следующим условиям:вес единицы товара более 5и кг либо объём единицы товара более 7и литров.
> SELECT COUNT(\*) FROM ANP.T_SQLT_ART WHERE WEIGHT > 5 OR VOL_TRANSP > 7

4. Вывести Кол-во записей в справочнике товаров, удовлетворяющих следующим условиям:вес единицы товара кратен 1 кг и находится в диапазоне от 1 до 3х кг и объем единицы товара в диапазоне от 3х до 5и литров.
> SELECT COUNT(\*) FROM ANP.T_SQLT_ART WHERE (WEIGHT %1 = 0 AND WEIGHT >= 1 AND WEIGHT <= 3) AND (VOL_TRANSP >= 3 AND VOL_TRANSP <= 5)

> Комментарий: При рассмотрении сам нашел ошибку в логическом операторе который написан не по условию задачи.

5. Вывести средний/минимальный/максимальный вес товара для каждой ГР21.Данные отсортировать по возрастанию полей ГР20-ГР21.Результаты округлить до 2х знаков после запятой.
> SELECT ART_GRP_LVL_0_NAME, ART_GRP_LVL_1_NAME, AVG(WEIGHT), MIN(WEIGHT), MAX(WEIGHT) ORDER BY ART_GRP_LVL_0_NAME, ART_GRP_LVL_1_NAME FROM ANP.T_SQLT_ART

6. Вывести список товаров из ГР22 - Сливки, которые поместятся в мешок объемом 1 литр.Для позиций из списка нужно рассчитать, сколько целых товаров поместится в этот мешок.Данные отсортировать по возрастанию поля Название товара.
> SELECT NAME, 1 / VOL_TRANSP FROM ANP.T_SQLT_ART WHERE VOL_TRANSP < 1 AND ART_GRP_LVL_2_NAME = "Сливки" ORDER BY NAME

> Комментарий: Не обратил внимание на условие со сливками, а по не опытности сделал ошибку в синтаксисе

7. Определить среднюю плотность(кг/л) товаров из справочника товаров.Результат округлить до 2х знаков после запятой.
> SELECT ROUND(WEIGHT/VOL_TRANSP, 2) FROM ANP.T_SQLT_ART

8. Выведите максимальный, средний и минимальный объём товаров из справочника товаров тремя разными строчками, отсортировав по убыванию.
> SELECT MAX(VOLUME) FROM ANP.T_SQLT_ART UNION ALL SELECT AVG(VOLUME) FROM ANP.T_SQLT_ART UNION SELECT MIN(VOLUME) FROM ANP.T_SQLT_ART
> 
> Комментарий: Мне указали на ошибку в выводе. Не понял как вывести построчно.

9. Выведите 5 товаров, набравших наибольшую суммарную выручку за 2018 годДанные отсортировать по убыванию значения суммарной выручки
> SELECT TOP 5 NAME, ART_GRP_LVL_0_NAME, SALE FROM ANP.T_SQLT_ART, ANP.T_SQLT_SALES WHERE ANP.T_SQLT_ART.ART_ID = ANP.T_SQLT_SALES.ART_ID

## Следующие задания были написаны совместно с помощником, я разобрался как строить эти запросы, но писал я их не сам. Для меня пока слишком тяжеловато. Но логику понял

10. Выведите 5 ГР21, имеющих наибольшую доходность(Выручка, руб - Себестоимость, руб) за весь период. Для полученных групп выведите остатки в рублях на 30 июня 2019 г. и 30 июня 2018 г. Данные отсортировать по убыванию значения доходности.
> select ART_GRP_LVL_0_NAME, ART_GRP_LVL_1_NAME, value from ANP.T_SQLT_ART as art
> join (select art_id, price as value
>                  from anp.t_sqlt_txn group by art_id) 
> as txn on art.art_id=txn.art_id
> order by value desc
> limit 5
> join (select ART_GRP_LVL_0_NAME, ART_GRP_LVL_1_NAME, value, rest19.rest_cp, rest18.rest_cp from ANP.T_SQLT_ART as art
> join (select art_id, sum(qnty*price) as value
> from anp.t_sqlt_txn group by art_id) 
> as txn on art.art_id=txn.art_id
> join (select art_id, rest_cp from anp.t_sqlt_rest where date = '2019-06-30') as rest19 on rest.art_id = art.art_id 
> join (select art_id, rest_cp from anp.t_sqlt_rest where date = '2018-06-30') as rest18 on rest.art_id = art.art_id
> order by value desc
> limit 5 )
