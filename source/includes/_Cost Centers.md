## Cost Centers 

### Get List of Cost Centers 

**Summary:** retrieve a list of cost centers

> Request example: Getting list of cost centers

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/users/cost-centers' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

> Response Example

```json
[
    {
        "id": 1,
        "name": "New anodot Cost Center",
        "code": "",
        "accountId": "932213950603",
        "linkedAccounts": [
            {
                "linkedAccountId": "247699517797",
                "linkedAccountName": "anodot-linked-account-test"
            },
            {
                "linkedAccountId": "835083597758",
                "linkedAccountName": "tests"
            },
            {
                "linkedAccountId": "932213950603",
                "linkedAccountName": "Pileus"
            }
        ]
    },
    {
        "id": 2,
        "name": "newCostCenterEdited",
        "code": "388",
        "accountId": "932213950603",
        "linkedAccounts": [
            {
                "linkedAccountId": "750699721763",
                "linkedAccountName": "Test"
            },
            {
                "linkedAccountId": "837447010640",
                "linkedAccountName": "reservation"
            }
        ]
    }
]
```

**Responses**

The response is comprised of an array of cost centers, according to the following table:

| Name | Type | Description | 
| ---- | ---- | ----------- | 
| id | ID | ID of the cost center |
| name | String | Cost Center Name | 
| code | String | Cost Center Code (optional) |
| accountId | ID | ID of the Anodot Cost Account | 
| linkedAccounts | Array | All the accounts linked to this cost center |

**Return Codes**

| Code | Description |
| ---- | ----------- |
| 200 | User retrieval success |
| 404 | User not found |
| 500 | Server error |

### Create Cost Center

**Summary:** Create a new cost center

> Request example: Create cost center

```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/users/cost-centers' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}' \
--data-raw '{
    "name": "SEs",
    "linkedAccountsIds": ["552822412346"],
    "code": "code blue"
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| name | body | Cost center name | yes | String |
| code | body | Cost center code | no | String |
| linkedAccountIds | body | All the linked accounts which should be attached to the cost center | Yes | Array |

> Response Example

```json
{
    "id": 11,
    "name": "SEs",
    "code": "code blue",
    "accountId": "932213950603",
    "linkedAccounts": [
        {
            "linkedAccountId": "959406495870",
            "linkedAccountName": "SE Playground"
        }
    ]
}
```

**Responses**

The response is comprised of an array of cost centers, according to the following table:

| Name | Type | Description |
| id | id | ID of the newly created cost center |
| cost center object | object | All the parameters which were passed to the create call | 




### Update Cost Center

**Summary:** Update an existing cost center

> Request example: Update cost center

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v1/users/cost-centers/{{costCenterId}}' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}' \
--data-raw '{
    "name": "SEs",
    "linkedAccountsIds": ["552822412346"],
    "code": "code blue"
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| costCenterID | path | ID of the cost center to update | Yes | String |
| name | body | Cost center name | yes | String |
| code | body | Cost center code | no | String |
| linkedAccountIds | body | All the linked accounts which should be attached to the cost center | Yes | Array |

**Responses**

The cost center object with the updated parameters. 

| Code | Description |
| ---- | ----------- |
| 200 | Cost center creation success |
| 404 | Error in parameters |
| 500 | Server error |


### Delete Cost Center

**Summary:** Delete a cost center

> Request example: Delete a cost center

```shell
curl --location --request DELETE 'api.mypileus.io/api/v1/users/cost-centers/{{costCenterId}}' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}' \
--data-raw ''
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| costCenterID | path | Cost center ID | yes | String |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Cost center Deletion successull |
| 404 | Error in parameters |
| 500 | Server error |
