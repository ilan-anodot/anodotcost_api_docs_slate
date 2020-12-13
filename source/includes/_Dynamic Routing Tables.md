# Dynamic Routing Tables

> End Point **/api/v2/dynamic-routing

Dynamic routing tables can be used in alerts to send alert triggers to specific email addresses according to a dimension value.

Use the dynamic routing end point to:

* Get the list of currently used tables in the account
* Get the content of a routing table
* Create a new routing table from a CSV file
* Update an existing table from a CSV file

Authentication type: [Access Token Authentication] (#access-tokens).

## GET Routing Tables

> Request Example: 

```shell
curl -X GET 'https://app.anodot.com/api/v2/dynamic-routing' \
-H 'Authorization: Bearer ${TOKEN}' \
-H 'Content-Type: application/json'
```

Get the list of routing tables in the account.
This request has no body

> Response Example:

```json
[
    {
        "id": "5faa7af2085f2bxxx1f41b99",
        "title": "dynamic routing demo 1.csv",
        "owner": "5f7033946b6a7exxxe3cf861",
        "creationTime": 1605008114,
        "editTime": null,
        "csv": "",
        "version": "1.0"
    },
    {
        "id": "5fa7ac804ef60dxxx10cac80",
        "title": "dynamic routing demo 1.csv",
        "owner": "5f917739c47f32b8cxxx44a9",
        "creationTime": 1604824192,
        "editTime": 1604902670,
        "csv": "",
        "version": "1.0"
    }
]
```

### Response Fields

The response is a list of the available routing tables used in the account</br>

Field | Type | Description / Example
-|-|-
id | String ($uuid) | The routing table uuid. Use this value to get information regarding a specific table.
title | String | The table name, this is the full name of the csv file that was used to create the table
owner | String ($uuid) | The id of The table owner in Anodot.
creationTime | epoch | Time the table was created in Anodot
editTime | epoch | Time the last update was done on the table in Anodot
csv | String | TBD
version | String | TBD

## GET Routing table

> Request Example: 

```shell
curl -X GET 'https://app.anodot.com/api/v2/dynamic-routing/{id}/csv' \
-H 'Authorization: Bearer ${TOKEN}' \
-H 'Content-Type: multipart/form-data'
```

Get a single table based on its id.

### Request Arguments

Replace {id} with the requested table id you received from the [GET Routing Tables] (#get-routing-tables) request.

> Response Example:

```json
[
    [ "US", "us_ae@myorg.com"],
    [ "UK", "uk_ae@myorg.com"]
]
```

### Response Fields

The routing table content.

Column 1 | Column 2
-|-
Dimension Value | Email destination(s)

## POST to upload a new table

> Request Example: 

```shell
curl --location --request POST 'https://app.anodot.com/api/v2/dynamic-routing' \
-H 'Authorization: Bearer ${TOKEN}' \
-H 'Content-Type: multipart/form-data' \
--form 'file=@/Users/myuser/Downloads/routing_table_sample.csv'
```

To upload a routing table, prepare a CSV file with the required routing rules and upload it as shown in this example.</br>
Make sure to include the ".csv" suffix in your call when you specify the file name to upload.

### Request Arguments

Argument | Type | Description
---------|------|------------
File Field | file reference | Specify the filename starting with @, and using the full path to the file location

<aside class="notice">
The CSV file format:</br>
- Do not use column headers. The first row is already a data row used for routing.</br>
- To route to several emails, separate them with a semi-colon.</br>
- Example:</br>
- "US", "us_ae1@myorg.com"; "us_ae2@myorg.com"</br>
- "UK", "uk_ae1@myorg.com"; "uk_ae2@myorg.com"</br>
</aside>

## POST to replace an existing table

> Request Example:

```shell
curl --location --request POST 'https://app.anodot.com/api/v2/dynamic-routing/{table_id}/csv' \
-H 'Authorization: Bearer ${TOKEN}' \
-H 'Content-Type: multipart/form-data' \
--form 'File=@/Users/myuser/Downloads/routing_table_sample.csv'
```

To update the information in an existing routing table, first update the information in a CSV using the same name, the upload it using the original table id.</br>
Make sure to include the ".csv" suffix in your call when you specify the file name to upload.

### Request Arguments

Argument | Type | Description
---------|------|------------
table_id | uuid |  Get the table id from the GET calls
File Field | file reference | Specify the filename starting with @, and using the full path to the file location