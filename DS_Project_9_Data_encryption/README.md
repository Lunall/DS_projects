# Проект 9. Шифрование данных и работа ML моделей

### Вводная: 
Наша компания озаботилась защитой данных при использовании моделей машинного обучения. Необходимо предложить такой метод защиты данных, чтобы по ним было сложно восстановить изначальные данные. Также необходимо рассмотреть то, как шифрование влияет на результат работы модели линейной регрессии, которая используется в компании.
    
Для проверки алгоритма предоставлены данные клиентов страховой компании «С» (пол, возраст и зарплата, количество членов семьи, количество страховых выплат). 

### Цель проекта 
 Предложить такой алгоритм защиты данных, при котором конечное качество моделей машинного обучения не ухудшится.

### Что было сделано:
**1.** На первом этапе мы загрузили и изучили данные.
- Для анализа были получены данные о 5000 клиентах. 
- Распределение целевого признака показывает что в основном клиенты не получают страховые выплаты (значение 0). Максимальное количество выплат - 5. Распределение похоже на распределение Пуассона.

- Пропусков в данных не было.

- Было найдено 153 дубликата. Их было решено оставить.

**2.** Был дан ответ на вопрос: Признаки умножают на обратимую матрицу. Изменится ли качество линейной регрессии? 
     Ответ: Не изменится. Мы в общем виде доказали что предсказания модели не поменяются. А также предоставили разбор того, как связаны параметры линейной регрессии в исходной задаче и в преобразованной.
        
**3.** Описали алгоритм преобразования данных и обосновали его.

**4.** На последнем шаге мы применили предложенный алгоритм.  
Сначала мы запустили модель без преобразований и посчитали R2 (R-Квадрат). 
С помощью `np.random` задали нашу матрицу-кодировщик K с правильной размерностью и обратной матрицей. 
Умножили признаки на матрицу К.
R2 score до и после преобразований не изменился и составлял: 0.4163.

