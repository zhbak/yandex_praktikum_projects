# Определение перспективного тарифа для телеком-компании

## Цели и задачи

**Цель –** определение выгодного тарифного плана для корректировки рекламного бюджета.

**Задачи:**
- обзор и предобработка данных;
- расчёт характеристик для каждого пользователя;
- анализ метрик;
- проверка гипотез:
    - равенства среднего выручек тарифов;
    - равенства среднего выручек Москвы и других регионов.

## Данные
Данные 500 пользователей тарифных планов «Смарт» и «Ультра» за 2018 год.

- Таблица users (информация о пользователях):
    - user_id — уникальный идентификатор пользователя
    - first_name — имя пользователя
    - last_name — фамилия пользователя
    - age — возраст пользователя (годы)
    - reg_date — дата подключения тарифа (день, месяц, год)
    - churn_date — дата прекращения пользования тарифом (если значение пропущено, то тариф ещё действовал на момент выгрузки данных)
    - city — город проживания пользователя
    - tariff — название тарифного плана

- Таблица calls (информация о звонках):
    - id — уникальный номер звонка
    - call_date — дата звонка
    - duration — длительность звонка в минутах
    - user_id — идентификатор пользователя, сделавшего звонок

- Таблица messages (информация о сообщениях):
    - id — уникальный номер сообщения
    - message_date — дата сообщения
    - user_id — идентификатор пользователя, отправившего сообщение

- Таблица internet (информация об интернет-сессиях):
    - id — уникальный номер сессии
    - mb_used — объём потраченного за сессию интернет-трафика (в мегабайтах)
    - session_date — дата интернет-сессии
    - user_id — идентификатор пользователя

- Таблица tariffs (информация о тарифах):
    - tariff_name — название тарифа
    - rub_monthly_fee — ежемесячная абонентская плата в рублях
    - minutes_included — количество минут разговора в месяц, включённых в абонентскую плату
    - messages_included — количество сообщений в месяц, включённых в абонентскую плату
    - mb_per_month_included — объём интернет-трафика, включённого в абонентскую плату (в мегабайтах)
    - rub_per_minute — стоимость минуты разговора сверх тарифного пакета (например, если в тарифе 100 минут разговора в месяц, то со 101 минуты будет взиматься плата)
    - rub_per_message — стоимость отправки сообщения сверх тарифного пакета
    - rub_per_gb — стоимость дополнительного гигабайта интернет-трафика сверх тарифного пакета (1 гигабайт = 1024 мегабайта)

## Примечание
- «Мегалайн» всегда округляет секунды до минут, а мегабайты — до гигабайт:
- Каждый звонок округляется отдельно: даже если он длился всего 1 секунду, будет засчитан как 1 минута. 
- Для веб-трафика отдельные сессии не считаются. Вместо этого общая сумма за месяц округляется в бо́льшую сторону. Если абонент использует 1025 мегабайт в этом месяце, с него возьмут плату за 2 гигабайта.

## Используемые инструменты: 
*Python, Pandas, Matplotlib, Seaborn, Numpy, Scipy*

## Выводы
**Обработка и исследовательский анализ данных:**
- Дубликы не были обнаружены.
- Пропусков в данных, которые надо было заменять на предварительном этапе не обнаружено.
- Выделены столбцы со значением календарного месяца (datetime) и номера месяца.
- Выполнены необходимые преобразования в формате данных.
- В большую сторону округлены минуты.
- Мегабайты переведы в гигабайты с учетом условий тарифов.
- Количество пользователей в данных по разным характеристикам несоответствовало их общему количеству в 500 пользователей, однако, в каждой таблице пользователи уникальны и имеются в списке клиентов `users`.
- На гистограмме продолжительности звонков наблюдается большое количество нулевых значений и как сказано в описании это пропущенные звонки.
- В датасете сообщений удалены клиенты с аномальным количеством сообщений в месяц.
- На гистограмме использованных мегабайт интернета в день выдеяются нулевые значения, которые могут быть связаны с тем, что клиент не пользовался мобильным интернетом в этот день.
- Устранены аномальные значения в таблице internet.
- После удаления аномалий проведен перерасчет помесячных значений характеристик.
- Рассчитана выручка.

**Главные выводы**
- Среднее значение длительности звонков и количества сообщений в тарифе ультра больше.
- Пользовтели тарифа ультра всегда укладываются в лимит сообщейни и длительности звонков.
- Значительная доля клиентов не пользуется сообщениями (много нулевых значений).
- Среднее значение использованных гигабайт в тарифе смарт выше.
- Дисперсия и среднеквадратическое отклонение у тарифа ультра больше по всем характеристикам.
- Ни в тарифе смарт, ни в тарифе ультра клиенты не укладываются в лимиты предоставленных за основную стоимость гигабайт.
- В лимит интернета смарта не укладывается бОлшая доля пользователей, чем в тарифе ультра.
- Среднее значение выручки тарифов ультра и смарт ИМЕЕТ статистически значимое различие.
- Средняя выручка пользователей из Москвы и пользователей из других регионов НЕ имеет статистически значимого различия.</span>
- Тариф смарт выгоднее тарифа ультра.

**Рекомендации**
- Рассмотреть выручку по характеристикам (звонки, сообщения, интернет).
- Рассмотреть выручку до лимитов и после - проверить насколько ощутима выручка с клиентов после исчерпания ими лимитов.
- *Для тарифа ультра:*
    -  Понизить лимит включенных в тариф количество минут до 1300 и сообщений до 160, а также повысить лимит включенных гигабайт до 80 (для удобства клиентов).
- *Для тарифа смарт:*
    -  Повысить лимит включенных в тариф количество минут до 900, сообщений до 110, гигабайт до 75 (для удобства клиентов).
- Осуществить односторонние гипотезы. С целью проверки статистически значимых различий средней выручки до и после внесенных рекомендуемых измененией в тариф.
- Осуществить анализ основных статистических характеристик тарифов и выручки по городам. Таких как среднее, медианное значение, различные перцентили, а также построение гистограмм с целью анализа различий между городами, оценки "портрета" регионального клиента, и возможной последующей корректировкой тарифов под регионы.
