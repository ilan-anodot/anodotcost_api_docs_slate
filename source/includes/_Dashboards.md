## Dashboards

> End Point **/api/v2/dashboards

Authentication type: [Access Token Authentication] (#access-tokens).

### Get Dashboards Count

> Request Example: 

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/dashboards/count' \
--header 'Authorization: Bearer {{bearer-token}}' \
--data-raw ''
```

Retrieves the number of dashboards defined in the account. 
This request has no body

#### Response Fields

> Response Example:

```json
{
    "dashboardsV1": 19,
    "dashboardsV2": 124
}
```

The response is the number of dashboards defined in the account, split into the two dashboard types - V1 and V2. 

Field | Type | Description / Example
-|-|-
dashboardsV1 | Number | Number of V1 dashboards defined in the account. 
dashboardsV2 | Number | Number of V2 dashboards defined in the account. 

Please note - when converting dashboards from V1 to V2, we internally store a V1 version of the dashboard for backwards compatability. So - in case old dashboards were converted, the V1 + V2 count will be *more* than the number shown in the UI. 

