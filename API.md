# Переход на 1.6
В рамках оптимизации Mercuryo переходит на версию api **1.6**. На данный момент поддерживается обратная совместимость запросов версии api **1.5**.

Для обновления версии api требуется изменить номер версии c **1.5** на **1.6**:

https://api.mercuryo.io/v1.5 -> https://api.mercuryo.io/v1.6

Большинство ответов остается без изменений.

# Обновленные ответы

Основное изменение -- ответ дополнился информацией о комисси:
1. `fee` -- комиссии
2. `subtotal` -- суммы без комиссии
3. `total` -- итоговые суммы с комиссией

# /widget/buy/rate
Запрос:

Пример запроса:
https://api.mercuryo.io/v1.6/widget/buy/rate?from=USD&to=BTC&amount=150&widget_id=d9d9dab5-7127-417b-92fb-478bc90916b3

Пример:

![buy/rate](https://github.com/IgnatBatuev/draft1.6api/blob/main/widget_buy.png)
# /widget/sell/rate
Запрос:

Пример запрос:
https://api.mercuryo.io/v1.6/widget/sell/rate?from=BTC&to=USD&amount=0.003&widget_id=d9d9dab5-7127-417b-92fb-478bc90916b3


Пример:

![buy/rate](https://github.com/IgnatBatuev/draft1.6api/blob/main/widget_sell.png)
# /public/convert
Запрос:

Пример запроса:
https://api.mercuryo.io/v1.6/public/convert?from=EUR&to=BTC&type=buy&amount=100&widget_id=d9d9dab5-7127-417b-92fb-478bc90916b3

Добавленные объекты в теле ответа
1. `rate` - курс криптовалюты на момент запроса
2. `fee_currency` - тип выбранного фиата

Пример:

![buy/rate](https://github.com/IgnatBatuev/draft1.6api/blob/main/widget_sell.png)
