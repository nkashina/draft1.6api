## 1. Silent Login

Silent login is an tool that makes login in Mercuryo easier for your users. You need to transfer users phone number to the Mercuryo API 

## 2. Acceptance: 

1. You need to add to your Terms and Policy agreement to share user data with us.
2. You need to make an agreement with Mercuryo that Mercuryo will use the data for registration and will use it to third parties
3. You need to ask users to accept  the Mercurio term  in your interface

## 3. API methods:
There is two API methods: one for users that already have mercuryo account and one for new ones

1. **Silent Login**

For already registered in Mercuryo users you need to use this API method:

Request: `GET https://api.mercuryo.io/v1.6/sdk-partner/login`

| Parameter  | Description  | 
| ------------- | -------------  |
| `phone` | users phone number |
| `Sdk-Partner-Token` | your widget ID |

Responses:

| Case  | Code  |  Response  |
| ------------- | -------------  |-------------  |
| user is register by you | 200 | api-token |
| user is register by other partner | 200 | next-token |
| unvalid phone | 400 |  unvalid phone |
| user is bloked by reasons: `LOCK_REASON_TOO_MANY_LOGIN_FAILURES`, `LOCK_REASON_FRAUD`, `LOCK_REASON_REFUND`, `LOCK_REASON_TOO_MANY_REQUESTS`, `LOCK_REASON_SANCTION_LIST`   | 403 | silent login forbidden |
| user is deleted `LOCK_REASON_DELETED` | 403 | silent login forbidden |
| unvalid `sdk-partner-token` | 401 | Wrong partner |
| user is not registred | 404 | user not found |


2. **Silent SignUp**

For users that have not Mercuryo account you need to use this API method:

Request: `GET https://api.mercuryo.io/v1.6/token/get-data`

| Parameter  | Description  | 
| ------------- | -------------  |
| `id` | your widget ID |
| `type` | type of your ID, there must be `sdk-partner` |

Response example:

`{

    "status": 200,
    
    "data": {
    
        "key": "5673a50719c7f17ca376b00dd62b039bfd2935200dd5880efbae5b49c3794227bG9naW4tdmVyaWZ5LXBob25lRZ6oBwVJVWSshXeKoL3fvMMKhLa7f8L7",
        
        "next": "verify-phone",
        
        "timeout": 24,
        
        "code_length": 4
        
    }
    
}`

**NB: token lifetime is 1 hour.**

If token is unvalid or expired:

Response example:

`{

    "name": "Not Found",
    
    "message": "Token not found or expired.",
    
    "code": 404000,
    
    "status": 404,
    
    "type": "yii\\web\\NotFoundHttpException"  
    
}`
