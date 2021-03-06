# Проект 7. Предсказание самого прибыльного месторождения

### Вводная: 
Увидев ваш сильный результат на прошедших соревнованиях с вами связалась крупная нефтедобывающая компания "Vankor Petroleum". В компании хотят автоматизировать процесс выбора скважин и региона для добычи нефти. В рамках пилотного проекта вам предстоит проанализировать синтетические данные о трех регионах и построить модель машинного обучения, которая определит тот регион, где добыча принесёт наибольшую прибыль.

### Цель проекта 
Построить модель для определения региона, где добыча принесёт наибольшую прибыль. Проанализировать возможную прибыль и риски.

### Что было сделано:
**1.** На первом шаге мы загрузили, проверили и обработали данные для дальнейшей работы с ними.  
1) Проверили пропуски в таблицах. 
2) Проверили типы данных.
3) Обработали дубликаты скважин.  
    *Возможная причина появления дубликатов* – были произведены повторные замеры признаков и старые данные по какой-либо причине не были удалены.  
    Так как мы не можем определить актуальные значения признаков, мы удаляли обе записи о скважинах. 
4) Исследовали корреляцию признаков.
    В регионе 0 наблюдалась отрицательная зависимость признаков "f0" и "f1" на уровне -0.44. При построении модели в этом регионе мы учли эту корреляцию.

**2.** На втором шаге мы выбрали наилучшую модель, предсказывающую количество продукта в скважине, а также предсказали средний запас сырья для каждого региона.

Результаты предсказаний модели:

Регион | Средний запас предсказанного сырья | RMSE | MAE модели
:---|---|---|---
Регион 0 | 92.6012 | 37.8541 | 31.1646
Регион 1 | 68.9174 | 0.8922 | 0.7201
Регион 2 | 95.1765 | 40.0905 | 32.8385

**3.** Далее мы провели подготовку к расчёту прибыли. 

Создали переменные с ключевыми значениями и составили формулу прибыльности для одной скважины:

`Бюджет на скважину <= Количество продукта в скважине * Доход с единицы продукта`

Мы получили, что в среднем в скважине должно быть как минимум 111 единиц продукта; бюджет на одну скважину составил 50 000 000 руб. Средний запас сырья не составлял 111 единиц ни в одном из регионов. Но поскольку мы будем брать 200 лучших скважин из 500, мы продолжили наши расчёты.

**4.** После мы написали функцию для расчёта прибыли по выбранным скважинам и предсказаниям модели. 

**5.** На последнем этапе были подсчитаны риски и прибыль для каждого региона.

Для этого мы построили распределение прибыли техникой Bootstrap с 1000 выборок по предсказаниям нашей модели.

1. 1000 раз модель отобрала 200 самых прибыльных скважин из случайных 500; 
2. Для каждого набора была рассчитана суммарная прибыль;
3. Для полученного распределения значений прибыли мы нашли среднее, 95%-й доверительный интервал и риски.

Оценка рисков в регионах показала, что при использовании модели вероятность убытков меньше 2.5% во всех регионах. 
Из них самая высокая средняя прибыль предсказана в регионе 1. 

<ins>Статистика по региону 1:</ins>  

Средняя прибыль для региона: 657 662 257 руб.  

95%-й доверительный интервал:   
Нижняя граница: 170 958 701 руб.  
Верхняя граница: 1 191 199 239 руб.

Риск убытков: 0.20%

Таким образом был выбран рекомендованный для разработки регион: **Регион 1.**