## Business Mapping
### Get Business Mappings 

**Summary:** Retrieves the business mapping rules defined in the account. 

**Description:** This call is used to retrieve the business mapping rules defined in an account. 

> Request Example: Getting business mappings


```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/usage/business-mapping/mappings' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}'
```

**Parameters:**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**
The response is an array of business mapping rules according to the following structure:


> Response Example 

```json
[
    {
        "updateTime": "2022-08-23 07:56:36",
        "split": false,
        "creationTime": "2022-08-04 15:37:35",
        "dbUpdateTime": "2022-11-23 15:19:11",
        "createdBy": "user@pileuscloud.com",
        "userKey": "ae853bda-2f0b-479d-85cc-226e61381cdc",
        "accountId": "932213950603",
        "name": "Dev",
        "rank": 1,
        "splitOptions": null,
        "uuid": "10f3351f-0780-4fe2-ae82-6647dda43c3b",
        "divisionId": "0",
        "accountKey": "21",
        "rules": [
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "env",
                        "values": [
                            "dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "Env",
                        "values": [
                            "dev",
                            "Dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "service",
                        "operator": "IS",
                        "values": [
                            "AWS Support [Business]",
                            "AWS Support [Developer]"
                        ]
                    }
                ]
            }
        ]
    },
]
```

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Create Business Mapping 

**Summary:** Create a new business mapping rule 

**Description:** This call is used to create a business mapping rule in the account. 

> Request Example: Creating a business mapping rule


```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/usage/business-mapping/mappings' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'  \
--header 'commonParams: {"isPpApplied":false}' \
--header 'Content-Type: application/json' \
--data-raw '
    {
        "name": "API",
        "rank": 38,
        "rules": [
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "env",
                        "values": [
                            "dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "Env",
                        "values": [
                            "dev",
                            "Dev"
                        ]
                    }
                ]
            }
        ],
        "split": "true",
        "splitOptions": {
        "mappings": [
                "92b818e1-33ce-495e-a39d-11a5862e11b7",
                "a5052a4d-1529-4680-8e89-22f890adb849",
                "545c005f-a185-453d-a56b-a2d9101ca2fd",
                "2f3138dc-3f5b-41bd-8d52-f2d39dcb754e",
                "fd0bda77-b275-40ef-9b21-7cd17b10df10",
                "c5e03b1d-ba33-4530-966b-bc91a8edf219"
            ]
        }
    }
```

**Parameters:**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| name | body | mapping name | Yes | string |
| rank | body | ranked order of this rule | Yes | Number |
| rules | body | Filters which define the mapping | Yes | Array of filter fields. See structure below |
| split | body | Determine whether the cost should be split | No | Boolean |
| splitOptions | body | Split type | No | One of "EQUAL", "RATIO" |
| mappings | body | Mapping ids to be split | No | Array of mapping IDs |

**Filter Structure**: Each filter has the following structure:

* "field": Name of filter field (e.g. service, customertags, etc.)
* "operator": one of IS, IS_NOT, STARTS_WITH, ENDS_WITH, LIKE, NOT_LIKE
* "values": should be an array when operator = IS, IS_NOT or a string otherwise
* "prefix": when field=customertags or k8slabels - this field should contain tag key


**Responses**
The response is the business mapping object created. 

> Response Example 

```json
[
    {
        "updateTime": "2022-08-23 07:56:36",
        "split": false,
        "creationTime": "2022-08-04 15:37:35",
        "dbUpdateTime": "2022-11-23 15:19:11",
        "createdBy": "user@pileuscloud.com",
        "userKey": "ae853bda-2f0b-479d-85cc-226e61381cdc",
        "accountId": "932213950603",
        "name": "Dev",
        "rank": 1,
        "splitOptions": null,
        "uuid": "10f3351f-0780-4fe2-ae82-6647dda43c3b",
        "divisionId": "0",
        "accountKey": "21",
        "rules": [
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "env",
                        "values": [
                            "dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "Env",
                        "values": [
                            "dev",
                            "Dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "service",
                        "operator": "IS",
                        "values": [
                            "AWS Support [Business]",
                            "AWS Support [Developer]"
                        ]
                    }
                ]
            }
        ]
    }
]
```

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Update Business Mapping 

> Request Example: Updating a business mapping rule

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v1/usage/business-mapping/mappings/{{businss-mapping-id}}' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}' \
--header 'Content-Type: application/json' \
--data-raw '
    {
        "name": "API",
        "rules": [
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "env",
                        "values": [
                            "dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "Env",
                        "values": [
                            "dev",
                            "Dev"
                        ]
                    }
                ]
            }
        ],
        "split": "true",
        "splitOptions": {
        "mappings": [
                "92b818e1-33ce-495e-a39d-11a5862e11b7",
                "a5052a4d-1529-4680-8e89-22f890adb849",
                "545c005f-a185-453d-a56b-a2d9101ca2fd",
                "2f3138dc-3f5b-41bd-8d52-f2d39dcb754e",
                "fd0bda77-b275-40ef-9b21-7cd17b10df10",
                "c5e03b1d-ba33-4530-966b-bc91a8edf219"
            ]
        }
    }
```

**Summary:** Update a business mapping rule

**Description:** This call is used to update a business mapping rule in the account. 

**Parameters:**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| business mapping id | header | ID of the business mapping to be updated | Yes | UUID |

Rest of the parameters are the same as the [Create Business Mapping call](#create-business-mapping).

> Response Example 

```json
[
    {
        "updateTime": "2022-08-23 07:56:36",
        "split": false,
        "creationTime": "2022-08-04 15:37:35",
        "dbUpdateTime": "2022-11-23 15:19:11",
        "createdBy": "user@pileuscloud.com",
        "userKey": "ae853bda-2f0b-479d-85cc-226e61381cdc",
        "accountId": "932213950603",
        "name": "Dev",
        "rank": 1,
        "splitOptions": null,
        "uuid": "10f3351f-0780-4fe2-ae82-6647dda43c3b",
        "divisionId": "0",
        "accountKey": "21",
        "rules": [
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "env",
                        "values": [
                            "dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "Env",
                        "values": [
                            "dev",
                            "Dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "service",
                        "operator": "IS",
                        "values": [
                            "AWS Support [Business]",
                            "AWS Support [Developer]"
                        ]
                    }
                ]
            }
        ]
    }
]
```

**Responses**
The response is the business mapping object created. 

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Delete Business Mapping 

**Summary:** Delete a business mapping rule

**Description:** This call is used to delete a business mapping rule in the account. 

> Request Example: Deleting  a business mapping rule


```shell
curl --location --request DELETE 'https://api.mypileus.io/api/v1/usage/business-mapping/mappings/{{businss-mapping-id}}' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}' \
--header 'Content-Type: application/json' \
--data-raw ''
```

**Parameters:**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| business mapping id | header | ID of the business mapping to be deleted | Yes | UUID |


**Responses**
The response is the business mapping object created. 

| Code | Description |
| ---- | ----------- |
| 200 | successful deletion |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Update Business Mapping Order

> Request Example: Updating a business mapping order

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v1/usage/business-mapping/mappings/rank' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}' \
--header 'Content-Type: application/json' \
--data-raw '--data-raw '[
    { 
        "uuid": "918029b6-0wellwwwf55-b8bf-6ce1f3983wek", 
        "rank": 1 
        },
    { 
        "uuid": "i39029b6-0161-4qwekq-b8bf-6ce1f3983sdk2", 
        "rank": 2 
        },
    { 
        "uuid": "019029b6-016lqoqoee55-b8bf-6ce1fqwemq3", 
        "rank": 3 
    }
]'
```

**Summary:** Update a business mapping order

**Description:** This call is used to update the order of the business mappings. 

**Parameters:**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| ranks | body | list of business mapping IDs with their new rank | Yes | Array

**Note:** You need to provide the full list of the mappings in the account for the ranking to work. 

> Response Example 

```json
[
    {
        "updateTime": "2022-08-23 07:56:36",
        "split": false,
        "creationTime": "2022-08-04 15:37:35",
        "dbUpdateTime": "2022-11-23 15:19:11",
        "createdBy": "user@pileuscloud.com",
        "userKey": "ae853bda-2f0b-479d-85cc-226e61381cdc",
        "accountId": "932213950603",
        "name": "Dev",
        "rank": 1,
        "splitOptions": null,
        "uuid": "10f3351f-0780-4fe2-ae82-6647dda43c3b",
        "divisionId": "0",
        "accountKey": "21",
        "rules": [
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "env",
                        "values": [
                            "dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "Env",
                        "values": [
                            "dev",
                            "Dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "service",
                        "operator": "IS",
                        "values": [
                            "AWS Support [Business]",
                            "AWS Support [Developer]"
                        ]
                    }
                ]
            }
        ]
    }
]
```

**Responses**
The response is the business mapping object created. 

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |
