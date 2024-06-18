## Business Mapping

### Get Viewpoints

**Summary:** Retrieves the viewpoints defined in the account. 

**Description:** This call is used to retrieve the viewpoints and the business mapping rule IDs within them.

> Request Example: Getting the viewpoints

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/usage/business-mapping/viewpoints' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}'
```

**Parameters:**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Response**

The response is an array of viewpoints and the business mapping rules defined within them.
Use the business mapping rule IDs to retrieve and update the rules.

> Response Example 

```json
[
    {
        "groups": [
            {
                "uuid": "8d1eb116-4f84-47ab-9fbb-89723e68f817",
                "rank": 1
            },
            {
                "uuid": "48d54258-2979-45bf-b614-93a3c76d5ec2",
                "rank": 2
            },
            {
                "uuid": "7b74169a-6ef2-4f99-a361-27658ded46e9",
                "rank": 3
            },
            {
                "uuid": "36056fd4-d1c6-4458-8f52-0ae76204ed7f",
                "rank": 4
            }
        ],
        "dbCreationTime": "2024-01-16 14:13:56",
        "k8s": false,
        "dbUpdateTime": "2024-03-28 07:43:24",
        "createdBy": "demo@anodotcost.com",
        "k8sConfig": {
            "podsView": false
        },
        "uuid": "ef0971e2-af04-4f7e-89bc-cdfb41b4f435",
        "divisionId": "0",
        "accountKey": "19",
        "userKey": "53bcbdd2-abee-4dad-b34f-d3c57c25225a",
        "accountId": "369369",
        "name": "Department"
    },
    {
            "groups": [
            {
                "uuid": "94f13b85-e76d-4fa9-bc3b-bfdc73c91142",
                "rank": 2
            },
            {
                "uuid": "82f5e1f2-3a07-460e-8317-9e0d246c6f7c",
                "rank": 3
            },
            {
                "uuid": "c875d3b2-4621-4c82-b653-9fadfcc79c6b",
                "rank": 4
            },
            {
                "uuid": "7903ac16-726e-4567-b37b-51ecaaa183e4",
                "rank": 1
            }
        ],
        "dbCreationTime": "2023-06-13 10:04:59",
        "k8s": false,
        "dbUpdateTime": "2023-08-27 13:59:45",
        "createdBy": "demo@anodotcost.com",
        "k8sConfig": {
            "podsView": false
        },
        "uuid": "fca7f91a-189b-46cf-91bb-f03346d4b9f9",
        "divisionId": "0",
        "accountKey": "19",
        "userKey": "b93b49a0-049d-4e45-a1db-93d0686c91d2",
        "accountId": "369369",
        "name": "Server Types"
    }
]
```

### Get Business Mapping by viewpoint ID 

**Summary:** Retrieves the business mapping rules by viewpoint ID. 

**Description:** This call is used to retrieve the business mapping rules defined in a specific viewpoint

<aside class="notice">
Get the viewpoint uuid by calling "Get Viewpoints" (see above)
</aside>

> Request Example: Use the viewpoint uuid in the request

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/usage/business-mapping/viewpoints/{enter-viewpoint-uuid}/mappings' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}'
```

> Response Example:

```json
[
    {
        "updateTime": "2024-05-15 05:18:57",
        "split": false,
        "creationTime": "2024-02-14 12:12:36",
        "k8s": false,
        "dbUpdateTime": "2024-05-15 05:18:57",
        "createdBy": "demo@anodot.com",
        "userKey": "53bcbdd2-abee-4dad-b34f-d3c57c25225a",
        "global": false,
        "filters": [
            {
                "index": 0,
                "field": "linkedaccid",
                "ruleId": 1,
                "operator": "IS",
                "values": [
                    "6892357334"
                ]
            },
            {
                "index": 1,
                "field": "customtags",
                "ruleId": 2,
                "operator": "IS",
                "prefix": "Environment",
                "values": [
                    "prod"
                ]
            },
            {
                "index": 2,
                "field": "familytype",
                "ruleId": 3,
                "operator": "IS",
                "values": [
                    "AWS Marketplace"
                ]
            },
            {
                "index": 3,
                "field": "resourceid",
                "ruleId": 4,
                "operator": "LIKE",
                "values": "prod"
            }
        ],
        "accountId": "369369",
        "name": "Production",
        "splitOptions": null,
        "k8sConfig": {
            "podsView": false
        },
        "uuid": "5a2f867b-03d1-4a4e-9367-246df2d2bf8e",
        "divisionId": "0",
        "accountKey": "19",
        "rank": 1
    },
    {
        "updateTime": "2024-02-19 12:37:28",
        "split": false,
        "creationTime": "2024-02-19 12:37:28",
        "k8s": false,
        "dbUpdateTime": "2024-02-19 12:37:28",
        "createdBy": "demo@anodot.com",
        "userKey": "53bcbdd2-abee-4dad-b34f-d3c57c25225a",
        "global": false,
        "filters": [
            {
                "index": 0,
                "field": "linkedaccid",
                "ruleId": 1,
                "operator": "IS",
                "values": [
                    "8323423411"
                ]
            },
            {
                "index": 1,
                "field": "resourceid",
                "ruleId": 2,
                "operator": "LIKE",
                "values": "dev"
            }
        ],
        "accountId": "369369",
        "name": "Dev",
        "splitOptions": null,
        "k8sConfig": {
            "podsView": false
        },
        "uuid": "1516e86b-9c1d-4f45-9236-91868fe21eb1",
        "divisionId": "0",
        "accountKey": "19",
        "rank": 2
    },
    {
        "updateTime": "2024-04-12 09:22:51",
        "split": false,
        "creationTime": "2024-04-12 09:22:51",
        "k8s": false,
        "dbUpdateTime": "2024-04-12 09:22:51",
        "createdBy": "demo@pileuscloud.com",
        "userKey": "b93b49a0-049d-4e45-a1db-93d0686c91d2",
        "global": false,
        "filters": [
            {
                "index": 0,
                "field": "linkedaccid",
                "ruleId": 1,
                "operator": "IS",
                "values": [
                    "6392651123"
                ]
            },
            {
                "index": 1,
                "field": "customtags",
                "ruleId": 2,
                "operator": "IS",
                "prefix": "Product",
                "values": [
                    "Robert Green"
                ]
            },
            {
                "index": 2,
                "field": "resourceid",
                "ruleId": 3,
                "operator": "LIKE",
                "values": "data"
            }
        ],
        "accountId": "369369",
        "name": "Data Project",
        "splitOptions": null,
        "k8sConfig": {
            "podsView": false
        },
        "uuid": "8c5bb278-e928-46e8-8828-3dba2d334c11",
        "divisionId": "0",
        "accountKey": "19",
        "rank": 3
    },
    {
        "updateTime": "2024-05-29 09:56:32",
        "split": false,
        "creationTime": "2024-05-29 09:56:32",
        "k8s": false,
        "dbUpdateTime": "2024-05-29 09:56:32",
        "createdBy": "demo@anodot.com",
        "userKey": "53bcbdd2-abee-4dad-b34f-d3c57c25225a",
        "global": false,
        "filters": [
            {
                "index": 0,
                "field": "service",
                "ruleId": 1,
                "operator": "IS",
                "values": [
                    "AWS Premium Support",
                    "AWS Support [Business]",
                    "AWS Support [Developer]",
                    "Support Business"
                ]
            }
        ],
        "accountId": "369369",
        "name": "Support",
        "splitOptions": null,
        "k8sConfig": {
            "podsView": false
        },
        "uuid": "7c95c6d1-4734-483d-8518-6ab7e2fcaaea",
        "divisionId": "0",
        "accountKey": "19",
        "rank": 4
    }
]
```

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
