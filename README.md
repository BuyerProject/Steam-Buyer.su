# Steam Buyer
Using the API, you can view the balance, create a deal to sell the balance, check the status.

## Authorization
Authorization goes with the help of a token consisting of 24 characters, this token can be obtained inside the telegram bot. The token is used for every request. Example:

`GET: https://steam-buyer.su/api/v1/user/balance?apikey=yVkkbrhJcat1b8wKnrbsFDKGYngTwdWy5VWZjeu9Fs0z7DOd`
    
### API Base URI
`https://steam-buyer.su/api/v1`
### Method
- GET
- POST

### Rate Limited
 - check balance, order - 100 in 60 sek
 - create order - 6 in 60 sek
 - 
#### Responses 429
    {
        "success":false,
        "body":{},
        "message":"Rate limit exceeded: 2 per 1 minute"
    }

#### Responses 422


    {
      "detail": [
        {
          "loc": [
            "string",
            0
          ],
          "msg": "string",
          "type": "string"
        }
      ]
    }


## User

### GET `/user/balance`

Balance and  hold balance

#### Successful Response

    {
    "success": true,
    "message": "",
    "body": {
        "balance": 0,
        "hold_balance": 0
        }
    }
Parameters:

 * apikey (required): Your key that you received inside the telegram bot

## Sell Order

### POST `/order/create`

Creating a balance sale request, the transaction may go to manual moderation, but basically everything happens automatically. When you create a deal, you receive an ID for tracking the status

#### Successful Response

    {
    "success": true,
    "message": "",
    "body": {
        "order_id": 0
        }
    }
Parameters:

 * steam_login_secure (required): A token for temporary access to your account
 * apikey (required): Your key that you received inside the telegram bot
 * referral_id (options): ID to receive a referral fee in the form of 5% of the amount received

A little bit about the referral system:

If you specified the referral's ID when creating the transaction (it must be specified at each creation), he will receive 5% of the amount received (after deducting the bot percentage). Your payment is charged to the user.

### GET `/order/{order_id}`

Get information about the transaction

#### Successful Response

    {
    "success": true,
    "message": "",
    "body": {
        "user_id": 0,
        "user_steam_id": 0,
        "amount_rub": 0,
        "user_wallet_iso": "EUR",
        "get_amount_rub": 0,
        "status": "moderation",
        }
    }
Parameters:

 * order_id (required): The ID of the transaction received during creation
 * apikey (required): Your key that you received inside the telegram bot

Transaction statuses:

* accept (active): The deal was approved by moderation
* active (active): The deal is in the process of being created
* cancel (inactive): The deal has been canceled
* error (inactive): The transaction was completed with an error
* moderation (wait): The deal is being moderated
* success (inactive): Successful balance transfer

Explanation of the error:

* invalid_login_secure: Your token is not valid
* community_ban: Your account has community_ban
* invalid_proxy: The error occurred on our side, please try again later
* less than the minimum amount: Your account balance is less than the minimum
* bot_not_place_item: The bot was unable to expose the item, please try again later
* bot_not_has_free_account: The bot has run out of accounts to accept the balance
* account_is_died: Your account died during the transfer
* unknown_error: The transaction ended with an unknown error

## That's all, if you have any questions, feel free to contact the bot support, we will be happy to help you set everything up
