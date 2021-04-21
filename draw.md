# Widget main info 
Widget is the most convenient way to integrate with Mercuryo. There are two ways to integrate - redirect and iframe
[FAQ](https://help.mercuryo.io/en/articles/4519473-mercuryo-widget-faq)

1. [How to start](/draw.md#1-how-to-start)

   1.1. [Step 1. Get parameters](//draw.md#11-step-1-.-get-parameters)
 
   1.2. [Step 2. Get a dashboard](//draw.md#12-step-2-.--get-a-dashboard)
 
   1.3. [Step 3. Set up a widget](//draw.md#13-step-3-.-set-up-a-widget)
 
   1.4. [Step 4. Check signature wallet address](//draw.md#14-step-4-.-check-signature-wallet-address)
2. [Webhooks](/draw.md#2-webhooks)
3. [Transaction status types ](/draw.md#3-transaction-status-types)

   3.1 [buy](//draw.md#31-buy)

   3.2 [sell](//draw.md#32-sell)
4. [API METHODS](/draw.md#4-api-methods)
5. [Signature Wallet Address](/draw.md#5-signature-wallet-address)
6. [Test](/draw.md#6-test)

   6.1. [SANDBOX](//draw.md#61-sandbox)

   6.2. [Get testnet transaction status](//62-get-testnet-transaction-status)
 
   6.3. [Get rates, limits etc](//draw.md#63-get-rates-limits-etc)

       

   6.4. [Check test transaction ](//draw.md#64-check-test-transaction)
	
***

#### 1. How to start

#### 1.1. Step 1. Get parameters

1. For integrate Mercuryo to your own platform use [**iframe**](https://demo-widget.mercuryo.io)

2. For redirection to Mercuryo platform use  [**redirect**](https://widget.mercuryo.io/docs.html). In this case partner get the comissions as with iframe.

#### 1.2. Step 2. Get a dashboard	
[Partners admin](https://partners.mercuryo.io)  

![img1](https://github.com/mercuryoio/api-migration-docs/blob/master/img1.png)

| Section  | Description  | 
| ------------- | -------------  |
| Dashboard | page with information abou transactions, also you can add widget by tap on <Add widget> button |
| My widgets | list of widgets |
| Widget callbacks | list of callbacks. You can send test callback from this page |
| Reports | log of Transactions, Referrals or Referrals Withdraw. You need to choose one of them to find the information|

#### 1.3 Step 3. Set up a widget
**My widgets → Create Partner Widget**

![img2](https://github.com/mercuryoio/api-migration-docs/blob/master/img2.png)

**Domain** &ndash; if Redirect `https://domain.io`, if iFrame your domain address (without “/” at the end of the address)
(1 widget = 1 domain)
**Note:** Please make sure, there are no symbols or backspace after `https://domain.io` if you choose Redirect or after `https://yourdomain.com` if you choose iFrame, otherwise the widget will not work properly and you’ll see `widget.mercuryo.io refused to connect` message.
**Backward URL** &ndash; merchant URL to which the users will return from Redirect
**Callback URL** &ndash; merchant server URL which listens to callbacks automatically when Mercuryo’s updates status of a transaction. 
 
#### 1.4. Step 4. Check signature wallet address

**Sign Key** &ndash; Callbacks signature check

When the status of a transaction changes, the partner receives a request with transaction data from Mercuryo. 

You can set-up a signature check and see the `X-Signature` HTTP header with a signature. 

You can generate a key here, for [example](https://implode.io/)

``` 
$key = '...';

$json = '{...}';

return hash_hmac('sha256', $json, $key) 
```

The signature can be checked by generating a hash through HMAC algorithm sha256 of the request body (JSON request) and a key that is in the partner's dashboard in the Sign Key field.

In this case, the request body must not change.

***

### 2. Webhooks	

These webhooks allow you to get current transaction status and include all the data.

1.  Set-up automatic webhooks to your server in a dashboard (`Callback URL`)  
2.  Before creating an order you should create a unique ID (max size 255 characters).
3.  Set the generated ID into the `merchantTransactionId` parameter (in let `widgetParams`) or in the URL parameter `merchant_transaction_id`.

`widget_id` &ndash; partners widget ID

`merchant_transaction_id` &ndash; generated unique ID by a partner, that was created in step 2	

#### 2.1. Callbacks signature check

**Sign Key** &ndash; Callbacks signature check

When the status of a transaction changes, the partner receives a request with transaction data from Mercuryo. 

You can set-up a signature check and see the `X-Signature` HTTP header with a signature. 

You can generate a key here, for [example](https://implode.io/)

``` 
$key = '...';

$json = '{...}';

return hash_hmac('sha256', $json, $key) 
```

The signature can be checked by generating a hash through HMAC algorithm sha256 of the request body (JSON request) and a key that is in the partner's dashboard in the Sign Key field.

In this case, the request body must not change.
		

#### 2.2. Callbacks Response Body
  
 Callbacks Payload example:
 
       ```JS
       {
    "payload": {
        "data": {
	"tx": {
	"id": "blockchain_transaction_id",
	"address": "blockchain_address"
	},
	"type": "withdraw",
	"user":{
	"uuid4":"mercuryo_user_uuid4,
	"country_code":"nz"
	},
	"amount": "0.02833",
	"status": "pending",
	"currency": "ETH",
	"created_at": "2021-03-09 20:05:20",
	"updated_at": "2021-03-09 20:05:20",
	"fiat_amount": "47.86",
	"created_at_ts": 1615320320,
	"fiat_currency": "USD",
	"updated_at_ts": 1615320320,
	"id":"mercuryo_id",
	"merchant_transaction_id":"merchant_transaction_id"
	}
       }
	```
| Parameter  | Description  | 
| ------------- | -------------  |
| `id` | unique ID of the current event |
| `uuid4` | unique user ID |
| `country_code` | code of users country |
| `number` | last 4 numbers of card number |
| `amount` | crypro amount |
| `status` | transaction status |
| `currency` | crypto currency |
| `created_at` and `updated_at` | data of start and last update |
|`fiat_currency` | fiat currency |
| `created_at_ts` | timestamp of creation |
| `fiat_currency` | code of fiat currency |
| `updated_at_ts` | timestamp of last update |
| `merchant_transaction_id` | merchant id |

***

### 3. Transaction status types 

#### 3.1. BUY
Per 1 transaction there are two internal operations "buy" and "withdraw"

**Type: `buy`**

| Status  | Description  | 
| ------------- | -------------  |
| `new` | transaction initiated |
| `pending` | waiting for any action from the user to continue the transaction (waiting for input 3ds or descriptor) |
| `cancelled` | transaction cancelled (usually due to timeout of descriptor or 3ds) |
| `paid` | transaction completed successfully (money debited from the card) |
| `order_failed` | transaction was rejected by the issuer bank |
| `order_scheduled` | transaction is successful, the money is held up/frozen on the card by the bank, we are waiting for the client to pass KYC. As soon as the client passes KYC - crypto is sent to the address, if the client fails KYC - transaction is canceled within 1 hours, client’s bank returns money back to card.|
|`descriptor_failed` | the user entered an invalid descriptor three times |

**Type: `withdraw`**

| Status  | Description  | 
| ------------- | -------------  |
| `new` | transaction initiated |
| `pending` | transaction in progress |
| `failed` | not completed successfully (this is rare) |
| `completed` | successfully (received transaction hash) |

#### 3.2. SELL

Per 1 transaction there are two internal operations "deposit" and "sell"

**Type:** `deposit`

| Status  | Description  | 
| ------------- | -------------  |
| `new` | new deposit |
| `pending`| deposit in progress |
| `succeeded` | deposit is done |
| `faild` | something gone wrong |

**Type:** `sell`

| Status  | Description  | 
| ------------- | -------------  |
|`new` | transaction created |
| `pending` | transaction in progress |
| `succeeded` | successfully completed (money transferred to the card) |
| `failed` | not completed successfully (crypto is refunded to `refund_address`) |

***

### 4. API METHODS 
			
#### 4.1. Get rates.

##### 4.1.1 rate+mercuryo fees+partners fee

Request:
`GET https://api.mercuryo.io/v1.6/public/rates`

Response example:
```js
"status": 200,
    "data": {
        "buy": {
            "ALGO": {
                "ARS": "124.98297107",
                "BRL": "7.42299956",
                "CHF": "1.22559999",
                "EUR": "1.10750000",
                "GBP": "0.95730000",
                "IDR": "19428.79347193",
                "ILS": "4.36429999",
                "JPY": "144.72785374",
                "KES": "133.79984881",
                "KRW": "1497.005988",
                "MXN": "26.62299785",
                "NGN": "549.28757402",
                "NOK": "11.10899917",
                "PLN": "5.07299996",
                "RUB": "102.12793771",
                "SEK": "11.27299889",
                "TRY": "10.80869932",
                "UAH": "37.75198981",
                "USD": "1.33240000",
                "ZAR": "19.03699822"
            }, ...
```


##### 4.1.2 rate+mercuryo fee

Request:
`https://api.mercuryo.io/v1.6/widget/rates/partner-fee-off`

[Example](https://api.mercuryo.io/v1.6/widget/rates/partner-fee-off?widget_id=d9d9dab5-7127-417b-92fb-478bc90916b3)

```js
"status": 200,
    "data": {
        "buy": {
            "BTC": {
                "EUR": "45999.85280047",
                "RUB": "4271678.76975651",
                "USD": "55253.19777882",
                "JPY": "6071645.41590771",
                "TRY": "449074.90569426",
                "GBP": "40221.37846708",
                "UAH": "1576044.12923561"
            }, ... ```
	 

##### 4.1.3 clear exchange rate

Request:
`https://api.mercuryo.io/v1.6/widget/rates/fee-off`

[Example](https://api.mercuryo.io/v1.6/widget/rates/fee-off?widget_id=d9d9dab5-7127-417b-92fb-478bc90916b3)


#### 4.2. Get transaction status

Request:
`https://api.mercuryo.io/v1.6/widget/transactions`

[Example](https://api.mercuryo.io/v1.6/widget/transactions?widget_id=d9d9dab5-7127-417b-92fb-478bc90916b3&merchant_transaction_id=1234)


#### 4.3. Get final crypto *buy* or fiat *sell* amounts**

##### 4.3.1. buy

Request:
`https://api.mercuryo.io/v1.6/public/convert`

[Example](https://api.mercuryo.io/v1.6/public/convert?from=EUR&to=BTC&type=buy&amount=100&widget_id=d9d9dab5-7127-417b-92fb-478bc90916b3)


##### 4.3.2. sell

Request:
`https://api.mercuryo.io/v1.6/public/convert`

[Example](https://api.mercuryo.io/v1.6/public/convert?from=BTC&to=EUR&type=sell&amount=0.1&widget_id=d9d9dab5-7127-417b-92fb-478bc90916b3)


##### 4.3.3. buy rate
Request:
`https://api.mercuryo.io/v1.6/widget/buy/rate`

[Example](https://api.mercuryo.io/v1.6/widget/buy/rate?from=USD&to=BTC&amount=100&widget_id=d9d9dab5-7127-417b-92fb-478bc90916b3)


#### 4.4. Get the list of supported fiat/cryptocurrencies

1. Request:
`https://api.mercuryo.io/v1.6/public/currencies-buy`

2. Request:
`https://api.mercuryo.io/v1.6/public/currencies-sell`	


#### 4.5. Get min/max limits				

##### 4.5.1 buy 
 
Request:
`https://api.mercuryo.io/v1.6/public/currency-limits`

[Example](https://api.mercuryo.io/v1.6/public/currency-limits?from=USD&to=BTC&widget_id=d9d9dab5-7127-417b-92fb-478bc90916b3&type=buy)


##### 4.5.2. sell 

Request:
`https://api.mercuryo.io/v1.6/public/currency-limits`

[Example:](https://api.mercuryo.io/v1.6/public/currency-limits?from=USD&to=BTC&widget_id=d9d9dab5-7127-417b-92fb-478bc90916b3&type=sell)	


#### 4.6. Get list of supported countries


Request:
`https://api.mercuryo.io/v1.6/public/card-countries` 

***

### 5. Signature Wallet Address

To protect against forgery of the crypto-wallet address, you have to use a signature.

On the dashboard, in the widget’s setting, there is a section where you can enable signature verification. 

The partner sets Secret (automatically generated or manually) and sets the “Check signature“ checkbox.


![img3](https://github.com/mercuryoio/api-migration-docs/blob/master/img3.png)

When the checkbox is on, the signature and address parameters must be appended to the widget’s URL (the partner generates on its side and substitutes).

Signature is calculated using the following algorithm:

signature = sha512(address+secret), where + concatenation operation.

If the signature is invalid, the widget will not be displayed.

Signature generation [example](https://abunchofutils.com/u/computing/sha512-hash-calculator)

![img4](https://github.com/mercuryoio/api-migration-docs/blob/master/img4.png)

***

### 6. TEST

You should provide all your test personal/server ip’s for whitelist to use Mercuryo’s sandbox. Contact your Mercuryo manager for it

How to use parameters

1. [iframe](https://demo-widget.mercuryo.io)
2. [redirect](https://widget.mercuryo.io/docs.html)		

#### 6.1. Mercuryo SANDBOX 

[Dashboard](https://sandbox-partners.mrcr.io) 

**Redirect**
 You can open widgetto [Sandbox](https://sandbox-exchange.mrcr.io) 

**iframe**

1. Place `<div id="mercuryo-widget"></div>` inside `<body></body>`
 
2. Put to widget HTML
```
<script src="https://sandbox-widget.mrcr.io/embed.2.0.js"></script>

<script>mercuryoWidget.run({widgetId: 

da128a17-d019-41f4-b80c-a615d7e0f595 

host: document.getElementById('mercuryo-widget')})

</script>
```

 in the end of the page before `</body>`
 
					
test `widget_id=60b69ef8-9287-49d7-8164-94d87d8982c4` You can find how to get your own test widget in second paragraph 
| Parameter name  | Description  | 
| ------------- | -------------  |
| phone | Phine number. You can use your own |
| email | E-mail adress. You can use your own |
| bank card | Bank card number. You can use your own real visa/MasterCard bank cards for 3ds part (the fiat won't be charged and data won't be stored) |


You can make your own test adresses in wallet. 

Here are examples of the test addresses:

test BTC address &ndash; `msBE6aCaAesegu4VzbQW3L5xWBL8vi15Q7`

test erc-20 address &ndash; `0xA14691F9f1F851bd0c20115Ec10B25FC174371DF`
				

#### 6.2. Check test transaction 	

[**BTC**](https://tbtc.bitaps.com) testnet

[**ETH**](https://ropsten.etherscan.io) ropsten, metamask
