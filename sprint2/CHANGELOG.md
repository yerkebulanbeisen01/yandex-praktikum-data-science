# Changelog

## 18.09.2019

✔️Пропуски в признаках `days_employed` и `total_income` заполнены на основе маппинга (образование, тип дохода) -> медианные значения.

✔️Скорректирован вывод о зависимости между уровнем дохода и возвратом кредита в срок.

### 17.09.2019

> 1. Удалять колонки с total_income NaN вот так вот сразу – достаточно лихое действие 🙂 Ты уверен, что хочешь удалять 10% датасета до сразу? Кажется, это требуется только для решения одной из следующих задач.
> 2. Я бы еще тут поисследовал, насколько случайны пропуски. Например, посмотрел бы как распределены пропуски в разных колонках.

Видимо мой мозг разработчика работает по принципу "меньше кода – меньше багов", но этот принцип не для данных 😀Как я писал ниже, пропуски не случайны и признаки `days_employed` и `total_income` взаимосвязаны. У меня было два варианта восстановления пропусков:

1. Заполнить пропуски 0
2. Заполнить пропуски медианой или средним значением по каждому из столбцов

✔ Т.к. я пока не знаю когда лучше применять второй вариант и насколько он окажется эффективным, то остановился на первом.

----

```python
# Определим основные категории целей кредита
purpose_category = [
    'свадьба',
    'недвижимость',
    'жилье',
    'автомобиль',
    'образование'
]
```

> Как получен этот список? Представь, что у тебя значительно больше уникальны причин – ты же не отсмотришь их все глазами. Здесь все-таки стоит придумать какой-то автоматический способ детектировать топовые леммы

✔️ Верно, если значений будет много, то станет проблематично быстро и без ошибок выделять категории. Альтернативным решением может быть поиск самых частовстречающихся слов в леммах и использование их в качестве категорий. Добавил этот момент.

----

> 2 – все-таки малодетная или многодетная? Кажется, код неоднозначный, дальше в .head() даже видно пример бага

✔ fixed: ключ для `малодетная` должен быть `2`. Кстати, пишут ли юнит-тесты для маленьких функций в рамках предобработки данных?

----

> Откуда здесь взяты числа для разделения? Тут лучше плясать от данных. Вдруг это какой-нибудь крайне специфический банк и там 99.9% с доходом <60к?

✔️Проверил разброс по доходу и выделил медиану. На основе этого сформировал категории.

----

> Шаг 3. Ответим на вопросы

✔️Скорректировал выводы используя сводные таблицы.