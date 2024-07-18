## Recommendations

We have announced Recommendations 2.0 in Anodot Cost's UI and API.</br>
The **Get Recommendations** request covers active and historical recommendations alike.

### Get Recommendations 2.0

> Request Example: Get recommendations 2.0 - get potential savings

```shell
curl --location --request POST 'https://api.mypileus.io/api/v2/recommendations/list' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}'\
--data '{
    "filters": {
    "open_recs_creation_date": {"from": "2024-01-01", "to": "2024-07-01"},
    "closed_and_done_recs_dates": {
    "last_update_date": {"from": "2024-01-01", "to": "2024-07-01"}
    },
    "status_filter": "potential_savings"
    },
    "sort": [ {
      "by": "savings",
      "order": "desc"
    }
    ],
    "page_size": 500,
    "pagination_token": null
}
```

**Summary:** Retrieves recommendations according to status and other filters

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| filters | body | see table below with all filters | Yes | json |
| sort | body | sorting order of the response | Yes | json array |
| page_size | body | max number of records to return in each page | Yes | number |
| pagination_token | body | The token should be copied from previous response to continue based on the pagination | Yes | string |

#### Status filters

> Filters example

```json
{
    "status_filter": "potential_savings",
    "user_status": {
        "DONE": true/false/null,
        "EXCLUDED": true/false/null 
        },
    "is_starred": true/false/null, 
    "open_recs_creation_date": {"from": "2024-01-13", "to": "2024-01-23"},
    "closed_and_done_recs_dates": {
        "last_update_date": { "from": "2024-01-13", "to": "2024-06-23" },
        "operator": AND/OR,
        "creation_date": { "from": "2024-01-13", "to": "2024-01-23" }
        },
    "annual_savings_greater_than": 1000,
    "cat_id": [1,2],
    "type_id": {
        "negate": true/false, //optional, default false 
        "eq": [ "rds-class-change", ...]
        },
    "service": {
        "negate": true/false, //optional, default false 
        "eq": [...]
        },
    "region": { // like type_id
        },
    "linked_account_id": {
    // like type_id
        },
    "instance_type": {
    // like type_id
        },
    "resource_id": {
    // like type_id
        },
    "virtual_tag": {
        "uuid": ...,
        "eq": [ "val1", ...]
        },
    "custom_tags": {
        "negate": true/false, //optional, default false
        "condition":
            [{
            "tag": "...",
            "eq": [ "val1", ...]
            }]
        },
    "enrichment_tags": {
    // like custom_tags
        } 
}
```

| Name | Description |
|------|-------------|
| status_filter | define what status of recommendations should be displayed. The possible values and meanings for this filter are:</br>* potential_savings - open AND undone AND not excluded</br>* actual_savings - (closed OR done) AND not excluded</br>* potential_and_actual_savings – all recommendations that were not excluded by the user</br>* excluded - excluded</br>* user_actions- done OR excluded</br>* custom - User defined conditions as set in the *is_open* and *user_status* fields, with logical AND between them. The *is_open* and *user_status* fields are ignored unless the *status_filter* is custom. |
| is_open  | true to return only open recommendations. false to return only closed. null or omit to return all |
| user_status | The 2 user statuses (done, excluded) as keys, and true/false/null as values.</br>* true - return only recommendations that have this status.</br>* false - return only recommendations that don’t have this status.</br>* Null or missing - do not filter by this status.</br> |

#### Date Filters

> Date filters example

```json
{
    "closed_and_done_recs_dates": {
        "last_update_date": {"from": ..., "to": ...},
        "operator": AND/OR,
        "creation_date": {"from": ..., "to": ...},
        }
}
```

There is a date filter for opened recommendations, and another date filter for closed and done recommendations. For open-done recommendations, both filters apply.

| Name | Applicable for | Description | 
|------|----------|-------------|
| open_recs_creation_date | Open | The filter for open recommendations allows filtering based on creation date:</br>{"from": "2024-01-13", "to": "2024-01-23"}</br>You may include the from or to fields or both |
| closed_and_done_recs_dates | Closed and Done | The filter for closed and done recommendations allows filtering based on update date, and optionally based on creation date. last_update_date is mandatory when using this filter.|

* last_update_date must include the from or to fields or both.
* operator and creation_date are optional and are added together. Either include both or omit both.
* creation_date must include the from or to fields or both.
* In all of these dates, the format is yyyy-MM-dd .
* The response will include recommendations within the dates {from to} - inclusive.

#### Tag Filters

> Tag Filters example

```json
"custom_tags": {
  "negate": false,
  "condition" : [
    {
        "tag": "env",
        "eq": [ "dev1", "dev2" ],
        "like": [ "test" ]
        "operator": "AND"
    },
    ...
  ]
}
```

> Virtual tags example

```json
"virtual_tag": {
  "uuid": "...",
  "eq": [ "...", "...", ... ],
  "like": [ "...", "...", ... ]
}
```

* custom_tags - static tags that relate to a specific resource, these tags are part of the recommendation raw data.
* virtual_tag - each virtual tag represents a collection of custom tags. 
* enrichment_tags - keys and values defined by you.

#### List Filters

These filters contain two fields:
* negate – true/false Default false. Determines include/exclude the values
* eq – An array of values

* type_id – The recommendation type  
* service
* region
* linked_account_id
* instance_type
* resource_id

#### Numeric Filters

* annual_savings_greater_than – Define the numeric value to be used as a filter, to get higher impacting recommendations
* cat_id – Filter by recommendation category, include one or more values in an array. See the values in the table below.

| cat_id | Description |
|--------|-------------|
| 1 | Right Sizing |
| 3 | Commitments |
| 4 | Terminate |
| 5 | Unattached |
| 7 | Generation Upgrade |
| 999999 | Other |

#### Response Fields

> Response example - Recommendation

```json
{
    "page": [
        {
            "recId": 43678022,
            "createdAt": "2024-06-15",
            "createdAtDateTime": "2024-06-15T00:00:00Z",
            "calculatedAtDateTime": "2024-07-15T06:02:29Z",
            "lastProcessingDate": "2024-07-14",
            "annualSavings": {
                "unblended": 3922.56,
                "amortized": 0,
                "netUnblended": 0,
                "netAmortized": 0
            },
            "monthlySavings": {
                "unblended": 326.88,
                "amortized": 0,
                "netUnblended": 0,
                "netAmortized": 0
            },
            "annualCurrentCost": {
                "unblended": 6151.68,
                "amortized": 0,
                "netUnblended": 0,
                "netAmortized": 0
            },
            "monthlyCurrentCost": {
                "unblended": 512.64,
                "amortized": 0,
                "netUnblended": 0,
                "netAmortized": 0
            },
            "age": 30,
            "cloudProvider": "AWS",
            "typeId": "rds-class-change",
            "typeName": "RDS Rightsizing",
            "linkedAccountId": "932213950603",
            "linkedAccountName": "Anodot Cost",
            "resourceId": "prod-recommendations-db",
            "instanceType": "db.m5.xlarge",
            "region": "us-east-1",
            "service": "RDS",
            "category": "Right Sizing",
            "recommendedAction": "modify",
            "customTags": {},
            "enrichmentTags": {
                "division": "Technology",
                "meta2": "Isarel",
                "meta1": "Rishon",
                "company": "Company A",
                "region": "US",
                "department": "R&D"
            },
            "starred": false,
            "comments": [
                {
                    "commentId": 5,
                    "createdBy": "ae853bda-2f0b-479d-85cc-226e61381cdc",
                    "createdAt": 1712122849,
                    "comment": "test2"
                }
            ],
            "open": true,
            "viewed": false,
            "userStatus": {
                "status": "done",
                "reason": null,
                "until": null,
                "period": null
            },
            "recData": { }
        }
    ],
    "isLastPage": false,
    "paginationToken": {
        "recId": 43678022,
        "annualSavings": {
            "unblended": 3922.56,
            "amortized": null,
            "netUnblended": null,
            "netAmortized": null
        }
    },
    "total": 423,
    "tableTotal": null
}
```

| Name | Description | 
| ---- | ----------- | 
| recId | Recommendation Id |
| createdAt | Recommendation creation date|
| createdAtDateTime | Recommendation creation date and time|
| calculatedAtDateTime | Recommendation calculation date and time |
| lastProcessingDate | The last date information for this recommendation was received |
| annualSavings | Calculated annual savings |
| monthlySavings | Monthly savings|
| annualCurrentCost | Annual current cost |
| monthlyCurrentCost | Monthly current cost |
| age | Recommendation age since creation |
| cloudProvider | AWS, Azure, GCP |
| typdId | Recommendation type Id in Anodot Cost |
| typeName | Recommendation type Name in Anodot Cost |
| linkedAccountId | Linked Account Id |
| linkedAccountName | Linked Account Name |
| resourceId | Resource included in the recommendation |
| instanceType | Resource type / family type |
| region | Resource related region |
| service | Resource related service |
| category | Anodot cost recommendation categories (see table above) |
| recommendedAction | The recommended action. |
| customTags | custom tags |
| enrichmentTags | enrichment tags |
| starred | Is the recommendation starred in Anodot Cost App. Values: true / false |
| comments | Recommendation comments in Anodot Cost App. |
| labels | Recommendation labels added in the Anodot Cost App. |
| open | Is the recommendation open. Values: true / false|
| viewed | Was the recommendation viewed by a user in Anodot Cost App. Values: true / false  |
| userStatus | * status: done/excluded/null</br>* reason: exclusion reason/null</br>* until: exclusion until date/null</br>* period: null (deprecated) |
| recData | Recommendation detailed information.</br>Details may differ according to the recommendation type and resource type.</br>The recommendation data will provide</br>* Data regarding current use</br>* Data regarding suggested use</br>* Recommendation alternatives if they exist |

> Response Example - recData section

```json
{
"recData": {
                "storage_type": "gp3",
                "recommended_cpu": 2,
                "recommended_instance_family": "General purpose",
                "iops_p95_data": {
                    "2024-06-30": 137.76,
                    .
                    .
                    "2024-07-07": 154.54
                },
                "mem_avg_data": {
                    "2024-06-30": 36.56,
                    .
                    .
                    "2024-07-07": 36.66
                },
                "cpu_max": 99.93,
                "mem_p95_data": {
                    "2024-06-30": 36.39,
                    .
                    .
                    "2024-07-07": 36.46
                },
                "zone_tag": "us-east-1",
                "ri_coverage": false,
                "recommended_memory": 8,
                "recommended_annual_cost": 2229.12,
                "cpu_p99_data": {
                    "2024-06-30": 13.88,
                    .
                    .
                    "2024-07-07": 9.74
                },
                "mem_max_data": {
                    "2024-06-30": 36.34,
                    .
                    .
                    "2024-07-07": 36.41
                },
                "iops_avg_data": {
                    "2024-06-30": 29.88,
                    .
                    .
                    "2024-07-07": 34.75
                },
                "db_connections_p95_data": {
                    "2024-06-30": 96.4,
                    .
                    .
                    "2024-07-07": 92.75
                },
                "db_connections_avg_data": {
                    "2024-06-30": 85.92,
                    .
                    .
                    "2024-07-07": 83.11
                },
                "cpu": 4,
                "rejected_reason_id": 0,
                "current_annual_cost": 6151.68,
                "mem_p95": 37.09,
                "cpu_p95": 14.49,
                "allocated_storage": 100,
                "resource_arn": "arn:aws:rds:us-east-1:932213950603:db:prod-recommendations-db",
                "alternatives": [
                    {
                        "memory": "8.0",
                        "monthly_cost": "185.76",
                        "potential_savings": "63.76",
                        "same_instance_type_family": false,
                        "instance_storage": "EBS Only",
                        "cpu": "2",
                        "hourly_cost": "0.258",
                        "physical_processor": "AWS Graviton2",
                        "command": "aws rds modify-db-instance --region us-east-1 --db-instance-identifier prod-recommendations-db --db-instance-class db.t4g.large --apply-immediately",
                        "cpu_max_estimated": {
                            "2024-06-30": 188.15,
                            .
                            .
                            "2024-07-07": 55.37
                        },
                        "mem_max_utilization_estimated": {
                            "2024-06-30": 72.68,
                            .
                            .
                            "2024-07-07": 72.83
                        },
                        "instance_type_family": "T4G",
                        "ri_coverage": true,
                        "annual_cost": "2229.12",
                        "is_multi_az": true,
                        "instance_family": "General purpose",
                        "instance_type": "db.t4g.large",
                        "saving_amount": "3923.0"
                    },
                    {
                        "memory": "8.0",
                        "monthly_cost": "208.8",
                        "potential_savings": "59.27",
                        "same_instance_type_family": false,
                        "instance_storage": "EBS Only",
                        "cpu": "2",
                        "hourly_cost": "0.29",
                        "physical_processor": "Intel Skylake E5 2686 v5 (2.5 GHz)",
                        "command": "aws rds modify-db-instance --region us-east-1 --db-instance-identifier prod-recommendations-db --db-instance-class db.t3.large --apply-immediately",
                        "cpu_max_estimated": {
                            "2024-06-30": 188.15,
                            .
                            .
                            "2024-07-07": 55.37
                        },
                        "mem_max_utilization_estimated": {
                            "2024-06-30": 72.68,
                            .
                            .
                            "2024-07-07": 72.83
                        },
                        "instance_type_family": "T3",
                        "ri_coverage": true,
                        "annual_cost": "2505.6",
                        "is_multi_az": true,
                        "instance_family": "General purpose",
                        "instance_type": "db.t3.large",
                        "saving_amount": "3646.0"
                    },
                    {
                        "memory": "8.0",
                        "monthly_cost": "228.96",
                        "potential_savings": "55.34",
                        "same_instance_type_family": false,
                        "instance_storage": "EBS Only",
                        "cpu": "2",
                        "hourly_cost": "0.318",
                        "physical_processor": "AWS Graviton2",
                        "command": "aws rds modify-db-instance --region us-east-1 --db-instance-identifier prod-recommendations-db --db-instance-class db.m6g.large --apply-immediately",
                        "cpu_max_estimated": {
                            "2024-06-30": 188.15,
                            .
                            .
                            "2024-07-07": 55.37
                        },
                        "mem_max_utilization_estimated": {
                            "2024-06-30": 72.68,
                            .
                            .
                            "2024-07-07": 72.83
                        },
                        "instance_type_family": "M6G",
                        "ri_coverage": true,
                        "annual_cost": "2747.52",
                        "is_multi_az": true,
                        "instance_family": "General purpose",
                        "instance_type": "db.m6g.large",
                        "saving_amount": "3404.0"
                    },
                    {
                        "memory": "8.0",
                        "monthly_cost": "242.64",
                        "potential_savings": "52.67",
                        "same_instance_type_family": false,
                        "instance_storage": "EBS Only",
                        "cpu": "2",
                        "hourly_cost": "0.337",
                        "physical_processor": "AWS Graviton3",
                        "command": "aws rds modify-db-instance --region us-east-1 --db-instance-identifier prod-recommendations-db --db-instance-class db.m7g.large --apply-immediately",
                        "cpu_max_estimated": {
                            "2024-06-30": 188.15,
                            .
                            .
                            "2024-07-07": 55.37
                        },
                        "mem_max_utilization_estimated": {
                            "2024-06-30": 72.68,
                            .
                            .
                            "2024-07-07": 72.83
                        },
                        "instance_type_family": "M7G",
                        "ri_coverage": true,
                        "annual_cost": "2911.68",
                        "is_multi_az": true,
                        "instance_family": "General purpose",
                        "instance_type": "db.m7g.large",
                        "saving_amount": "3240.0"
                    },
                    {
                        "memory": "8.0",
                        "monthly_cost": "256.32",
                        "potential_savings": "50.0",
                        "same_instance_type_family": true,
                        "instance_storage": "EBS Only",
                        "cpu": "2",
                        "hourly_cost": "0.356",
                        "physical_processor": "Intel Xeon Platinum 8175",
                        "command": "aws rds modify-db-instance --region us-east-1 --db-instance-identifier prod-recommendations-db --db-instance-class db.m5.large --apply-immediately",
                        "cpu_max_estimated": {
                            "2024-06-30": 188.15,
                            .
                            .
                            "2024-07-07": 55.37
                        },
                        "mem_max_utilization_estimated": {
                            "2024-06-30": 72.68,
                            .
                            .
                            "2024-07-07": 72.83
                        },
                        "instance_type_family": "M5",
                        "ri_coverage": true,
                        "annual_cost": "3075.84",
                        "is_multi_az": true,
                        "instance_family": "General purpose",
                        "instance_type": "db.m5.large",
                        "saving_amount": "3076.0"
                    }
                ],
                "days_to_check": 60,
                "cpu_p99": 39.61,
                "memory": 16,
                "throughput_max_data": {
                    "2024-06-30": 38370979.58,
                    .
                    .
                    "2024-07-07": 27794169.52
                },
                "max_memory": 37.04,
                "max_allocated_storage": 100,
                "throughput_avg_data": {
                    "2024-06-30": 862022.88,
                    .
                    .
                    "2024-07-07": 918150.19
                },
                "network_avg_data": {
                    "2024-06-30": 1117750.81,
                    .
                    .
                    "2024-07-07": 1159182.59
                },
                "network_p95_data": {
                    "2024-06-30": 4659980.28,
                    .
                    .
                    "2024-07-07": 5326768.59
                },
                "throughput_p95_data": {
                    "2024-06-30": 3482309.3,
                    .
                    .
                    "2024-07-07": 3996757.08
                },
                "engine": "PostgreSQL",
                "recommended_monthly_cost": 185.76,
                "is_multi_az": 1,
                "cpu_max_data": {
                    "2024-06-30": 94.08,
                    .
                    .
                    "2024-07-07": 27.69
                },
                "recommended_instance_type_family": "T4G",
                "db_connections_max_data": {
                    "2024-06-30": 105,
                    .
                    .
                    "2024-07-07": 97
                },
                "max_iops": 4767.93,
                "cpu_p95_data": {
                    "2024-06-30": 6.49,
                    .
                    .
                    "2024-07-07": 7.8
                },
                "physical_processor": "Intel Xeon Platinum 8175",
                "network_max_data": {
                    "2024-06-30": 31450749.09,
                    .
                    .
                    "2024-07-07": 31391230.34
                },
                "instance_type_family": "M5",
                "command_comment": "Before initiating an AWS RDS resizing, it is crucial to verify backup availability and minimize downtime to prevent any adverse effects on your application's availability.It’s important to acknowledge that a brief period of downtime might occur during the switch-over process.",
                "iops_max_data": {
                    "2024-06-30": 845.46,
                    .
                    .
                    "2024-07-07": 825.15
                },
                "cpu_avg_data": {
                    "2024-06-30": 3.84,
                    .
                    .
                    "2024-07-07": 3.88
                },
                "engine_version": "15.5",
                "instance_family": "General purpose",
                "recommended_instance_type": "db.t4g.large"
            }
}
```

### Deprecated - Get Recommendations

**Summary:** Retrieves current live recommendations

**Description:** The call is used to retrieve the current live recommendations

> Request Example: Getting recommendations

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/recommendations?filters[type]=ec2-idle' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}'
```


**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| filters | query | Filter recommendations based on type | No |  |

Filtering recommendations can be applied to the above calls using query parameters in the following manner:**filters[type]=recommendationType** where recommendationType is one of the recommendation types supported. You can use the *Get recommendation types* call to get the possible types.</br>

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

> Response Example: Recommendation

```json
[
    {
        "max_cpu_util": "0.8",
        "region": "us-east-1",
        "operation_system": "Linux/UNIX",
        "resource_name": "mongodb-rs1.k8s-production.anodot.com",
        "total_cost_recommended": "0",
        "resource_tags": {
            "createdBy": "AssumedRole:AROAIFFBAISDNBXFCKHYE:tcohen@anodot.com",
            "Resource_Name": "mongodb-rs1.k8s-production.anodot.com",
            "stack": "production"
        },
        "initial_creation_date": "2022-06-12",
        "resource_id": "i-0fbec9ade54277c5f",
        "linked_account_name": "Production",
        "signature": "340481513670#|#340481513670#|#ec2-idle#|#i-0fbec9ade54277c5f",
        "total_cost_current": "389.86",
        "sp_coverage": "0.0",
        "instance_type_size": "2xlarge",
        "network_out": 1,
        "recommendation_creation_time": "2022-06-30 22:34:25",
        "max_network": 10,
        "ri_coverage": "100.0",
        "network_in_statistics_usage": {
            "2022-06-08": "0.22"
        },
        "sp_savings": false,
        "instance_type_model": "r5ad",
        "starting_time": "2022-06-30 22:34:04",
        "last_process_date": "2022-06-29",
        "num_of_days": 30,
        "action": "Terminate",
        "cpu_util_statistics_usage": {
            "2022-06-08": "0.6683333333333333"
        },
        "account_id": "340481513670",
        "ri_savings": true,
        "linked_account_id": "340481513670",
        "service": "Amazon Elastic Compute Cloud",
        "network_in": 1,
        "recommendation_update_time": "2022-06-30 22:34:25",
        "uuid": "92ab0dcc-5dee-4df2-b186-4b246293b3c8",
        "potential_savings": "100.0",
        "recommendation_status_id": 0,
        "network_out_statistics_usage": {
            "2022-06-08": "1.3"
        },
        "type": "ec2-idle"
    }
]
```

While each recommendation has different attributes that relate to the recommendation, they all share the following common fields:

| Name | Description | 
| ---- | ----------- | 
| account_id | The payer account id |
| linked_account_id | The linked account id that the recommendation is relevant to (can be the payer account id also) |
| linked_account_name | The linked account name that the recommendation is relevant to (can be the payer account name also) |
| type | The recommendation type (See Recommendations Types Below) |
| total_cost_current | The corresponding monthly cost without executing the recommendation |
| total_cost_recommended | The corresponding monthly cost after executing the recommendation |
| potential_savings | The monthly savings percent in case the recommendation is executed |
| action | The general type of action related to the recommendation - Modify/Terminate/Upgrade/Delete/Buy |
| recommendation_creation_time | The time the recommendation was created |
| uuid | The recommendation id |
| recommendation_status_id |The recommendation status (See Recommendations Statuses Below) |
| recommendation_update_time | The time the recommendation was updated with one of the statuses. |

**Recommendation Statuses**

Available Recommendation Statuses are: 

* Open Recommendation (Recommendation which no action have been made on it): Type ID: 0
* Completed Recommendation: Type ID: 1
* Excluded Recommendation (Recommendation which excluded action has been made on it): Type ID: 4

### Get Recommendations types

**Summary:** Retrieves current live recommendations types

**Description:** The call is used to retrieve the current live recommendations types

Example:

> Request Example: Getting recommendations types

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/recommendations/types' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**

> Response Example: Recommendation Types

```json
[
    "ebs-unattached",
    "ip-unattached",
    "ec2-idle",
    "dynamodb-idle",
    "version-upgrade",
    "rds-version-upgrade",
    "ec2-low-cpu-usage",
    "rds-ri",
    "ebs-upgrade",
    "elasticache-ri",
    "ec2-savings-plans",
    "nat-gateway-util-low",
    "ebs-outdated-snapshot",
    "rds-idle",
    "ec2-stopped-instance"
]
```
| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### DEPRECATED - Get history of recommendations

<aside class="warning">
This request is deprecated and will be replaced in the future
</aside>

> Request Example: Getting recommendations history

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/recommendations/history?filters[type]=ec2-idle' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}'
```

**Summary:** Retrieves the history of recommendations

**Description:** The call is used to retrieve historical recommendations</br>

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| filters | query | Filter recommendations based on type | No |  |

Filters are the same as the live [Get Recommendations](#get-recommendations) call

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### NEW - Get history of recommendations

> Request Example: Getting recommendations history by date range

```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/recommendations/history?filters[type]=ec2-idle' \
--header 'Authorization: {{bearer-token}}' \
--header 'apikey: {{account-api-key}}' \
--header 'commonParams: {"isPpApplied":false}' \
--header 'Content-Type: application/json' \
--data '{
    "startDate": "2023-07-28 00:00:00",
    "endDate": "2023-08-27 23:59:59",
    "limit":"all"
}'
```

**Summary:** Retrieves the history of recommendations

**Description:** The call is used to retrieve historical recommendations</br>
The call supports pagination and it is recommended to use it with a date range.

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| filters | query | Filter recommendations, see table below | No | Strings from list of values |
| limit | Body | Overall number of records to retrieve. Use "all" to get all responses without pagination | Yes | Number |
| pageSize | Body | Number of records to return in each response. Default and Maximum is 80 entries. | No | Number |
| startDate | Body | Start date of the queried period. No default value. | No | 'YYYY-MM-DD hh:mm:ss' |
| endDate | Body | End date of the queried period. No default value. | No | 'YYYY-MM-DD hh:mm:ss' |
| lastEvaluatedKey | Body | The primary key of the last retrieved recommendation | No | {account_id, uuid} |

<aside class="success">
To filter by recommendation type use <b>filters[type]=recommendationType</b></br>
recommendationType is one of the recommendation types returned by the <b>Get recommendation types</b> call.</br>
The type are also listed in the recommendation documenation.
</aside>

**Filters**

| String | Description | Example | 
|-|-|-|
| region | The region related to the recommendation | us-east-1 |
| resource_id | The resource id related to the recommendation | export-schemas-poc |
| account_id | The payer account id | 114780013227 |
| linked_account_id	| The linked account id that the recommendation is relevant to | 114780013227 |
| type | Recommendation type | s3-idle |
| service | The service name related to the recommendation | Amazon Simple Storage Service |
| action | The general type of action related to the recommendation | Modify/Terminate/Upgrade/Delete/Buy |
| uuid | The recommendation id | 0034f235-fce2-418b-b4d4-215e2c0d6d89 |
| recommendation_status_id | The recommendation status | Completed - 1, Excluded - 4, Completed by system - 6 |



**Response fields**

| Name | Description |
| -- | -- |
| data | The recommendation data records |
| lastPageKey | The primary key of the last retrieved recommendation {account_id, uuid} |



### Get recommendation history types

**Summary:** Retrieves current history recommendations types

**Description:** The call is used to retrieve the history recommendations types

> Request Example: Getting recommendations history types

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/recommendations/history/types' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

