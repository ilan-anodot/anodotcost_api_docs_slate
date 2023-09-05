## Reseller Credits
### Get Credits

**Summary:** Retrieves credits

**Description:** The call is used to retrieve credits by a reseller

> Request Example: Getting credits

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/divisions/customers/aws/credit' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

> Response Example: Credit

```json
[
    {
        "id": "48956495_DIVISION-NAME",
        "name": "Test Credit",
        "description": null,
        "accountId": "123456789012",
        "divisionName": "DIVISION-NAME",
        "amount": 1000,
        "monthlyAmountLimit": 100,
        "amountUsed": 300,
        "startDate": "2023-01",
        "expirationDate": "2023-03",
        "dedicatedParams": "[{\"type\":\"awsMarketplace\",\"relation\":\"exclude\",\"values\":[\"AWS Marketplace\"]}]",
        "isExcludeAwsMarketplace": true,
        "service": {
            "relation": "include",
            "values": "all"
        }
    }
]
```

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 401 | Unathorized |
| 500 | Server error |

### Add Credit

> Request Example: Adding credit for list of customers

```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/divisions/customers/aws/credit' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
--data '{
    "name": "Test Credit",
    "description": "Test credit",
    "divNames": ["CUSTOMER1", "customer2"],
    "amount": 1000,
    "monthlyAmountLimit": 300,
    "startDate": "2023-01-01",
    "expirationDate": "2023-02-28",
    "service": ["SERVICE_NAME_1", "SERVICE_NAME_2"],
    "isExcludeService": true,
    "isExcludeAwsMarketplace": false,
}'
```

**Summary:** Add credit for customers

**Description:**

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| name | body | the name of credit | Yes | string |
| description | body | credit description | No | string |
| divNames | body | customers list that will get the credit | Yes | array of strings |
| amount | body | credit total amount | Yes | number |
| monthlyAmountLimit | body | Optional monthly limit | No | number |
| startDate | body | Format "YYYY-MM-DD" | Yes | string |
| expirationDate | body | Format "YYYY-MM-DD" | Yes | string |
| service | body | Set the services the credit will apply to | Yes | array of strings (see note below) |
| isExcludeService | body | set list of service filter as exclude filter | Yes | boolean |
| isExcludeAwsMarketplace | body | To exclude AWS Marketplace services | Yes | boolean |

<aside class="notice">
To set all services, the <i>service</i> parameter value should be an array with the string "all" ["all"].
</aside>

> Response Example: Add credit

```json
[
    {
        "id": "abcd1234_CUSTOMER1",
        "accId": "123456789012",
        "name": "Test Credit",
        "description": "Test credit",
        "amount": 1000,
        "monthlyAmountLimit": 300,
        "startDate": "2023-01-01",
        "expirationDate": "2023-02-28",
        "service": ["SERVICE_NAME_1", "SERVICE_NAME_2"],
        "isExcludeService": true,
        "isExcludeAwsMarketplace": false,
        "amountUsed": 0,
        "divisionName": "CUSTOMER1"
    },
    {
        "id": "abcd1234_customer2",
        "accId": "123456789012",
        "name": "Test Credit",
        "description": "Test credit",
        "amount": 1000,
        "monthlyAmountLimit": 300,
        "startDate": "2023-01-01",
        "expirationDate": "2023-02-28",
        "service": ["SERVICE_NAME_1", "SERVICE_NAME_2"],
        "isExcludeService": true,
        "isExcludeAwsMarketplace": false,
        "amountUsed": 0,
        "divisionName": "customer2"
    }
]
```

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 401 | Unauthorized |
| 500 | Server error |


### Delete credit

> Request Example: Delete credit

```shell
curl --location --request DELETE 'https://api.mypileus.io/api/v1/divisions/customers/aws/credit/abcd1234_customer2' \
--header 'Authorization: {{bearer-token}}' \
--header 'apikey: {{account-api-key}}' \
--header 'Content-Type: application/json'
```

**Summary:** Delete credit

**Description:** Delete specific credit by id

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| id | query param | credit ID | Yes | string |

<aside class="notice">
Bear in mind that deleting a credit item deletes all the alerts linked to it.
</aside>

**Responses**

> Response Example: Delete credit
```json
{
    "id": "abcd1234_customer2"
}
```

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 401 | Unauthorized |
| 500 | Server error |
