# Lookup Tables

> End Point **/api/v2/lookup/data**

Lookup tables can be used in data streams to replace dimension values and/or filter records according to the dimension value.

Use the lookup end point to:

* Create a new table from a CSV file
* Update an existing table from a CSV file

Authentication type: [Access Token Authentication] (#access-tokens).

## Get lookup tables

> Request Example: 

```shell
curl -X GET \
https://app.anodot.com/api/v2/lookup/data \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Get the list of lookup tables currently available at this account.

> Response Example:

```json
[
    "lookup1.csv",
    "lookup2.csv",
    "simple-lookup6.csv",
    "simple-lookup7.csv",
    "simple-lookup8.csv",
    "simple-lookup9.csv",
    "simple-lookup11.csv"
]
```

### Response Fields

The response is a list of the available lookup tables used in the account

## POST / PUT lookup table

This endpoint supports two ways to load the CSV files

1. Binary file stream - Use this method if you plan to provide the file content as a binary stream of data.
2. Form data - Use this method if you plan to provide your users with a User Interface to submit the file.

<aside class="notice">
The common way to upload a file with an API would be the binary stream of data.<br/>
The csv file must reside in the destination specified by the "data-binary" parameter.<br/>
If the file is not found, an empty entry will be created in Anodot.
</aside>

> Request Example: Using binary file stream

> The requested file name should appear as the filename parameter 

```shell
curl -X POST \
https://app.anodot.com/api/v2/lookup/data?filename=mylookup.csv \
-H 'Content-Type: text/csv' \
-H "Authorization: Bearer ${TOKEN}" \
--data-binary '@/Users/myuser/filesforAnodot/mylookup.csv'
```

### Request Arguments - Binary file stream

Argument | Type | Description
---------|------|------------
filename | string | The file name used to store the binary data stream information
data-binary| file reference | The binary stream of data. The exmple shows a cURL reference according to a file name, by using the "@" symbol at the start of the string.

> Request Example: Using form-data

```shell
curl -X POST \
https://app.anodot.com/api/v2/lookup/data \
-H 'Content-Type: multipart/form-data' \
-H "Authorization: Bearer ${TOKEN}" \
-F 'filefield=@mylookup.csv'
```

### Request Arguments - Form data

Argument | Type | Description
---------|------|------------
File Field | file reference | Specify the filename starting with @

<aside class="notice">
Remember:</br>The POST and PUT requests have an identical format.<br/>
Use POST to create a new table - the name of the file you use must be unique.<br/>
Use PUT to update an existing table - the name of the file you use must match an existing table in the account.
</aside>
