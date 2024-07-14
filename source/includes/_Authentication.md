## Cost Authentication

In order to start working with the Cost API, you first need to perform three steps.

1. Get the Authentication token
2. Call the USERS API to get the account and division IDs
3. Form the **{{account-api-key}}** by combining the token from step 1 with the account and division from step 2.

See details below

### Getting the Token

> Request example: Getting an authentication token

```shell
curl --location --request POST 'https://tokenizer.mypileus.io/prod/credentials' \
--header 'Content-Type: application/json' \
--data-raw '{
    "username": "coyote@acme.com",
    "password": "thisismypassword"
}'
```

The following call will enable you to get an authentication token for the subsequent API calls.
The token is valid for 24 hours.

You can either use basic authentication (recommended) or send the parameters in the body.

> Response example - token

```json
{
    "Authorization": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",  
    "apikey": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX:-1" 
}
```

> Response example - invalid credentials.

```json
{
    "Error": "Failed Retrieving Authentication Token"
}
```

### Getting the account and division

> Sample reponse from the users API

```json
[
  {
    "accounts": [
        {
            "accountKey":"{{accountkey}}",
            "accountId": "1234567",
            "accountTypeId": 1,
            "accountName": "anodot",
            "cloudTypeId": 0,
            "userKey": "{{user-key}}",
            "firstProcessTime": "2020-09-22T12:00:00.000Z",
            "lastProcessTime": "2022-06-30T12:00:00.000Z",
            "divisionId": "{{divisionId}}",
            "divisionsIds": [
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0
            ]
        }
  }
]
```

After calling the Authentication, you will need to call the [Get Users](#users).
From the users API response you will get an array of accounts (See example on the right)

### Forming the API Key

Choose the **accountkey** which is relevant for the subsequent API calls and the **divisionID**. 

Then - replace the '-1' at the end of the API key you got with accountKey:divisionId

Example: API key from step 1:  *c6249e30e-cf36-4957-a41a-d08fbafa5093:-1*

Will be changed to: *c6249e30e-cf36-4957-a41a-d08fbafa5093:{{acountKey}}:{{divisionId}}*

In the following calls, we refer to this new string as **{{account-api-key}}**
