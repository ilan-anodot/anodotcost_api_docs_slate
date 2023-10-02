## MSP Customers

### Get MSP Customers

**Summary:** Retrieves MSP customer list

**Description:** The call is used to retrieve the list of customers for an MSP

> Request Example: Get MSP Customers

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/msp/customers' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**

> Response Example: 

```json
[
    {
        "customerName": "xyz",
        "customerCode": "",
        "customerId": 999,
        "linkedAccountIds": [
            "111111111111"
        ],
        "customerNameId": "xyzw"
    },
    {
        "customerName": "abc",
        "customerCode": "abcdef",
        "customerId": 9999,
        "linkedAccountIds": [
            "222222222222"
        ],
        "customerNameId": "abc"
    }
]
```

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 401 | Unauthorized request |
| 500 | Server error |

The response is an array of customers linked to this MSP

**Response Fields**

| Field | Type |
| ----- | ----------- |
| customerName | String |
| customerCode | String (can be empty) |
| customerId | Integer |
| linkedAccountIds | list of strings |
| customerNameId | String |

<aside class="notice">
The <i>customerCode</i> field can be an empty string</br>
The <i>customerName</i> and <i>customerNameId</i> fields may be identical, or different.</br>
The <i>customerNameId</i> should be used in following calls like update and delete.
</aside>

### Create an MSP Customer

**Summary:** Create a new MSP customer

**Description:** The call is used to create a new customer for an MSP

> Request Example: Create MSP Customer

```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/msp/customers' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "customerName",
    "linkedAccountsIds": [“19923818823”, “342134123”],
    "code": "customerCode"
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| name | Body | The customer name | Yes | String |
| linkedAccountsIds | Body | List of linked accounts for this customer | Yes | Array of Strings |
| code | Body | An additional customer identifier | No | String |

**Responses**

> Response Example

```json
{
    "customerName": "xyz",
    "customerCode": "",
    "customerId": 999,
    "linkedAccountIds": [
        "111111111111"
    ],
    "customerNameId": "xyz"
}
```

| Code | Description |
| ---- | ----------- |
| 201 | successful Creation |
| 400 | Invalid Parameters value |
| 500 | Server error |

The response contains the created customer information

<aside class="notice">
The <i>customerNameId</i> field is identical to the <i>customerName</i> used in the create request.</br>
While the <i>customerName</i> can be altered in the future, the <i>customerNameId</i>remains fixed.
</aside>

### Edit MSP Customer

**Summary:** Update an MSP customer

**Description:** The call is used to update an MSP customer

> Request Example - Edit an MSP customer

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v1/msp/customers/fd50e8f0-823a-456e-9df8-7e14f8df3fb7' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "customerName",
    "linkedAccountsIds": [“19923818823”, “342134123”],
    "code": "customerCode"
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| id | path | The customer ID to update | Yes | String |

**Responses**

The response will contain the updated customer in the regular structure.

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Delete MSP Customer

**Summary:** delete an MSP customer

**Description:** The call is used to delete an MSP customer

> Request Example: Delete MSP Customer

```shell
curl --location --request DELETE 'https://api.mypileus.io/api/v1/msp/customers/fd50e8f0-823a-456e-9df8-7e14f8df3fb7' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw ''
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| id | path | The customer Id to remove | Yes | String |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful deletion |
| 400 | Invalid Parameters value |
| 500 | Server error |
