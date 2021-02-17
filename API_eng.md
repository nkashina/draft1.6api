# Switching to 1.6
As part of optimization Mercuryo is switching to api version * 1.6 *. At the moment old version is supported. After the update the methods configured for the api * 1.5 * version will continue supported.

You need to change the version number from 1.5 to 1.6 to update:

https://api.mercuryo.io/v1.5 -> https://api.mercuryo.io/v1.6/

Most of the responses remain unchanged. The list of changed pesponses is below

# Updated responses
# widget / buy / rate
https://api.mercuryo.io/v1.6/widget/buy/rate?from=USD&to=BTC&amount=150&widget_id=d9d9dab5-7127-417b-92fb-478bc90916b3

Added fields in response body:
1. `fee` - commission
2. `subtotal` - amount without commission
3. `total` - total amount with commission

Example:

![buy/rate](https://github.com/IgnatBatuev/draft1.6api/blob/main/widget_sell.png?raw=true)
# widget / sell / rate
https://api.mercuryo.io/v1.6/widget/sell/rate?from=BTC&to=USD&amount=0.003&widget_id=d9d9dab5-7127-417b-92fb-478bc90916b3

Added fields in response body:
1. `fee` - commission
2. `subtotal` - amount without commission
3. `total` - total amount with commission

Example:

![buy/rate](https://github.com/IgnatBatuev/draft1.6api/blob/main/widget_sell.png)
# public / convert
https://api.mercuryo.io/v1.6/public/convert?from=EUR&to=BTC&type=buy&amount=100&widget_id=d9d9dab5-7127-417b-92fb-478bc90916b3

Added fields in response body
1. `rate` - exchange rate of selected crypto to selected fiat
2. `fee` - commission
3. `fee_currency` - type of selected fiat

Example:

![buy/rate](https://github.com/IgnatBatuev/draft1.6api/blob/main/public_convert.png)
