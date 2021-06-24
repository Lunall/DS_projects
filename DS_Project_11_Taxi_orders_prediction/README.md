# Проект 11. Прогнозирование заказов такси

### Вводная 
Компания, предоставляющая сервис заказов такси Clean Taxi собрала исторические данные о заказах такси в аэропортах. Для того, чтобы привлекать больше водителей в часы пик и управлять ценообразованием, вам как аналитику нужно построить модель, которая будет предсказывать количество заказов такси на следующий час.

### Цель проекта 
​	Построить модель для предсказания количества заказов такси на следующий час. Значение метрики RMSE на тестовой выборке должно быть не больше 48.

### Что было сделано:
**1.** На первом этапе мы получили и обработали данные. Всего было 26496 наблюдений, записи шли каждые 10 минут.  
Период наблюдения: (с 2018-03-01 по 2018-08-31)

1.1. Была проведена подготовка данных для дальнейшей работы:
- Проверили данные на пропуски, дубликаты и монотонность.
- Провели ресемплирование временного ряда. Поскольку наша задача – предсказать количество заказов на следующий час, ресемплирование мы проводили по одному часу.

1.2. В конце мы добавили календарные признаки, а также создали функцию, которая позволяет задать длину лага и ширину окна для скользящего среднего.

**2.** На данном этапе мы проводили исследовательский анализ данных.

Временной ряд был разобран на составляющие:

- `Тренд` График тренда показывал нам, что количество заказов растёт со временем. Если в марте было около 60 заказов, то к концу августа их стало под 170. Единственная проблема заключается в том, что у нас нет сезонных данных о годах в целом – неизвестно как будут вести себя значения с наступлением осени.

- `Сезонность` Сезонная составляющая отобразила рисунок спроса на такси в рамках 1-го дня. "Провалы" на графике скорее всего отображали ночной период, а пики – вечерний.

- `Остаток декомпозиции` Мы наблюдали что с увеличением количества заказов увеличивался и разброс значений остатков.

В конце мы построили распределение целевого признака.
Его распределение было похоже на нормальное – с пиком значений около 60 заказов, смещением графика влево и небольшим хвостом значений справа.

**3.** Третий этап был посвящен обучению моделей.

Процесс обучения был построен следующим образом: тестовую выборку мы задавали размером 10% от исходных данных. Отставание (lag) мы задавали от 0 до 10-и дней. Для кросс-валидации мы использовали TimeSeriesSplit()

3.1. В начале мы рассмотрели работу модели Линейной регрессии.  

Мы получили график зависимости RMSE от величины лага, который мы создаем для анализа. После значения в 72 часа rmse начинает колебаться в пределах 24 - 22, поэтому решили остановиться на отставании в 168 часов.
Лучший результат на данном этапе: rmse = 22.45.

3.2. Ridge  

Далее мы решили добавить регуляризацию в нашу модель и выбрали тип регуляцизации наиболее подходящий для нашей задачи.
Поскольку все наши признаки созданы из "одного", то скорее всего значимость признаков будет распределена между всеми более-менее равномерно, а в этой ситуации L2 должна показывать себя лучше. Таким образом мы пришли к выводу, что лучше использовать L2 (Ridge) регуляризацию.

Регуляризация помогла немного улучшить результат: RMSE на обучающей выборке уменьшилось до 22.13.

3.3. RandomForestRegressor  

Потом мы решили рассмотреть как показывает себя модель Случайного леса в этой задаче.  
Лучший результат на данном этапе: rmse = 22.95  
Результат был немного, но хуже чем у предыдущих моделей.

3.4. LGBM

Далее мы попробовали использовать Градиентный бустинг из модуля LightGBM.
Лучший результат на данном этапе: 22.95
Модель обучалась очень долго. 6 минут на обучение с кросс-валидацией без перебора гиперпараметров. Результат такой же как и у модели случайного леса.

Итоговая таблица результатов:

| Модель                 | Лучшая **RMSE** |
| :--------------------- | --------------- |
| **Линейная регрессия** | 22.45           |
| **Ridge**              | 22.13           |
| **Случайный лес**      | 22.95           |
| **LightGBM**           | 22.95           |

**4.** На последнем этапе мы провели анализ лучшей модели.

Мы протестировали модель Ridge регрессии, поскольку она показала лучшие результаты.  
Также, для лучшей интерпретируемости оценки, мы решили добавить расчет MAE.

    RMSE на тестовой выборке: 35.47
    МАЕ на тестовой выборке: 26.10

Изначальной цели в RMSE менее 48 нам удалось достичь.

МАЕ в 26.10 говорит нам о том, что наша модель в среднем ошибается на 26 заказов. В целом это неплохой результат: можно будет предсказывать необходимое количество таксистов с точностью около 15% в пиковое время заказов (когда их число достигает 200).

В конце мы проверили модель на адекватность.

Итоговая RMSE dummy модели, не использующей машинное обучение, составила 86.41. Как видим, наша модель предсказывает гораздо точнее: её RMSE на тестовой выборке составляло 35.47.
