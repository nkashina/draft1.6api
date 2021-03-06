# Переход на 1.6
В рамках оптимизации Mercuryo переходит на версию api **1.6**. На данный момент поддерживается обратная совместимость запросов версии api **1.5**.

Для обновления версии api требуется изменить номер версии c **1.5** на **1.6**:

https://api.mercuryo.io/v1.5 -> https://api.mercuryo.io/v1.6

Большинство ответов остается без изменений.

# Обновленные ответы

Основное изменение &ndash; ответ дополнился информацией о комиссии:
1. `fee` &ndash; комиссии
2. `subtotal` &ndash; суммы без комиссии
3. `total` &ndash; итоговые суммы с комиссией
4. `rate` &ndash; курс криптовалюты на момент запроса

# /widget/buy/rate
Запрос:
https://api.mercuryo.io/v1.6/widget/buy/rate?from={fiat_currency}&to={crypto_currency}&amount={from_amount}&widget_id={widget_id}

Пример запроса:

https://api.mercuryo.io/v1.6/widget/buy/rate?from=USD&to=BTC&amount=150&widget_id=d9d9dab5-7127-417b-92fb-478bc90916b3

Пример:

![buy/rate](https://github.com/IgnatBatuev/draft1.6api/blob/main/buy_comparev3.png)
# /widget/sell/rate
Запрос:

https://api.mercuryo.io/v1.6/widget/sell/rate?from={crypto_currency}&to={fiat_currency}&amount={from_amount}3&widget_id={widget_id}

Пример запрос:

https://api.mercuryo.io/v1.6/widget/sell/rate?from=BTC&to=USD&amount=0.003&widget_id=d9d9dab5-7127-417b-92fb-478bc90916b3


Пример:

![buy/rate](https://github.com/IgnatBatuev/draft1.6api/blob/main/sell__comparev3.png)
# /public/convert
Запрос:

https://api.mercuryo.io/v1.6/public/convert?from={fiat_currency}&to={crypto_currency}&type={type}&amount={from_amount}&widget_id={widget_id}

Пример запроса:

https://api.mercuryo.io/v1.6/public/convert?from=EUR&to=BTC&type=buy&amount=100&widget_id=d9d9dab5-7127-417b-92fb-478bc90916b3

Добавленные объекты в теле ответа
1. `fee_currency` &ndash; тип выбранного фиата

Пример:

![public/convert](https://github.com/IgnatBatuev/draft1.6api/blob/main/conver_comparev3.png)


| Status  | Meaning  | 
| ------------- | -------------  |
| `new` | transaction initiated |
| `pending` | waiting for any action from the user to continue the transaction (waiting for input 3ds or descriptor) |
| `cancelled` | transaction cancelled (usually due to timeout of descriptor or 3ds) |
| `paid` | transaction completed successfully (money debited from the card) |
| `order_failed` | transaction was rejected by the issuer bank |
| `order_scheduled` | transaction is successful, the money is held up/frozen on the card by the bank, we are waiting for the client to pass KYC. As soon as the client passes KYC - crypto is sent to the address, if the client fails KYC - transaction is canceled within 1 hours, client’s bank returns money back to card.|
|`descriptor_failed` | the user entered an invalid descriptor three times |



1. [Get rates](/Widget_API_Mercuryo_v1.6.md/#1-api-methods/#41-get-rates)

   1.1. [rate mercuryo fees partners fee](/Widget_API_Mercuryo_v1.6.md/#11-rate-mercuryo-fees-partners-fee)
 
   1.2. [rate mercuryo fee](/Widget_API_Mercuryo_v1.6.md/#12-rate-mercuryo-fee)
 
   1.3. [clear exchange rate](/Widget_API_Mercuryo_v1.6.md/#13-clear-exchange-rate)
2. [Get transaction status](/Widget_API_Mercuryo_v1.6.md/#2-get-transaction-status)
3. [Get final crypto *buy* or fiat *sell* amounts ](/Widget_API_Mercuryo_v1.6.md/#3-get-final-crypto-buy-or-fiat-sell-amounts)

   3.1 [buy](/Widget_API_Mercuryo_v1.6.md/#31-buy)

   3.2 [sell](/Widget_API_Mercuryo_v1.6.md/#32-sell)
4. [Get the list of supported fiat or crypto currencies](/Widget_API_Mercuryo_v1.6.md/#4-get-the-list-of-supported-fiat-or-crypto-currencies)
5. [Get min or max limits](/Widget_API_Mercuryo_v1.6.md/#5-get-min-or-max-limits)

   5.1 [buy](/Widget_API_Mercuryo_v1.6.md/#51-buy)

   5.2 [sell](/Widget_API_Mercuryo_v1.6.md/#52-sell)
6. [Get list of supported countries](/Widget_API_Mercuryo_v1.6.md/#6-get-list-of-supported-countries)
