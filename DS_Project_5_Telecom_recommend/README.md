# Проект 5. Рекомендация тарифов

### Вводная: 
Вы продолжаете работать в компании сотового оператора «ВодаФон». Отдел аналитики выяснил, что многие клиенты используют невыгодные для компании устаревшие тарифы. Сегодня поступила задача начать переводить пользователей на новые тарифы, первая стадия – проанализировать поведение клиента и на его основе предложить рекомендуемый тариф.

Для построения рекомендательной системы вам предоставлены уже обработанные данные (из `Проекта 3. Определение перспективного тарифа для телеком компании`) о поведении клиентов, которые уже пользуются тарифами «Смарт» и «Ультра».

На данном этапе руководство хочет видеть как можно большее значение *accuracy* (доля правильных ответов), она должна быть по крайней мере 0.75. 

### Цель проекта 
Нужно построить модель для задачи классификации, которая выберет подходящий тариф. Доля правильных ответов должна быть по крайней мере 0.75.

#### Описание данных
Каждый объект в наборе данных — это информация о поведении одного пользователя за месяц. Известно:

Обозначение | Признак
:---|:---
сalls | количество звонков,
minutes | суммарная длительность звонков в минутах,
messages | количество sms-сообщений,
mb_used | израсходованный интернет-трафик в Мб,
is_ultra | каким тарифом пользовался в течение месяца («Ультра» — 1, «Смарт» — 0).