## Divisions
### Get Divisions 

<aside class="warning">
The Get Divisions API has been deprecated. Please use the Get Cost Centers API instead.
</aside>

**Summary:** retrieve divisions

> Request Example: Get Divisions

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/divisions' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

> Response Example: TBD


**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Divisions retrival success |
| 404 | Divisions not found |
| 500 | Server error |
