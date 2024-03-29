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

Balance and hold balance

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
Additional authorization method:
Instead of steam_refresh_token you can pass two parameters store_steam_login_secure && community_steam_login_secure Steam has separated access tokens. Or you can simply pass steam_refresh_token.

 * steam_refresh_token (required): A token for temporary access to your account login.steampowered.com
 * apikey (required): Your key that you received inside the telegram bot
 * referral_id (options): ID to receive a referral fee in the form of 5% of the amount received
 * send_telegram_message (options): Allows you to disable messages in telegram

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
        "buyer_steam_id": 0,
        "amount_rub": 0,
        "get_amount_rub": 0,
        "to_receive": 0,
        "user_wallet_iso": "EUR",
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
* not_a_unique_account: The account was uploaded by another user. You must try again in 5 minutes
* wrong_format_steam_login_secure: Invalid token format
* community_ban: Your account has community_ban
* invalid_proxy: The error occurred on our side, please try again later
* less_than_the_minimum_amount: Your account balance is less than the minimum
* bot_not_place_item: The bot was unable to expose the item, please try again later
* bot_not_has_free_account: The bot has run out of accounts to accept the balance
* account_is_died: Your account died during the transfer
* unknown_error: The transaction ended with an unknown error
* an_insecure_account: The account has a high chance of being banned
* family_view: Parental controls are set on the account

## Other

### GET `/other/currency`

Receiving quotes that are used in the bot, updated every 6 hours

#### Successful Response

    {
    "success": true,
    "message": "",
    "body": {
        "timestamp": 0,
        "base": "EUR",
        "rates": {}
        }
    }
Parameters:

 * apikey (required): Your key that you received inside the telegram bot


## That's all, if you have any questions, feel free to contact the bot support, we will be happy to help you set everything up
