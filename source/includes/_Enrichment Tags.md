## Enrichment tags

Mapping the organizational structure in filters, recommendations and CUE is made available by uploading the file via API.
Your enrichment tags are stored as a single csv file.
To update the CSV file using the API, you need to:

1. GET the current configuration file
2. In a text editor - Update the entries as needed
3. POST the updated file to replace the existing configuration in your account

### GET Enrichment tags

> Request example: GET enrichement tags file

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/users/files/enrichment-tags/download' \
--header 'apikey: {{account-api-key}}' \
--header 'authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}' \
--header 'Content-Type: text/plain' \
```

Use this request to download the current file to your local drive.
The only parameter for this request is the account api key.

> Response example:

```json
{
    "metadata": {
        "id": "489538b6-xxx",
        "fileType": "text/csv",
        "accountKey": "xx",
        "userKey": "82f99b85-e85a-436e-a8eb-d4ed989b9456",
        "accountId": "987654321098",
        "fileName": "replacement-filenamewwww2.csv",
        "uploadDate": "2023-02-09 12:21:35"
    },
    "data": [
        {
            "department": "Marketing",
            "company": "Company A",
            "region": "EU",
            "division": "Sales",
            "linked_account_id": "1111111111"
        },
        {
            "department": "R&D",
            "company": "Company A",
            "region": "US",
            "division": "Technology",
            "linked_account_id": "222222222"
        },
        {
            "department": "Budget",
            "company": "Company A",
            "region": "EU",
            "division": "Finance",
            "linked_account_id": "33333333333"
        }
    ]
}
```

### POST Enrichment tags

> Request Example: POST enrichment tags file

```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/users/files/enrichment-tags/upload' \
--header 'apikey: {{account-api-key}}' \
--header 'authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}' \
--data_raw '{
    {
    "metaData": {
        "type": "text/csv",
        "name": "tags.csv"
    },
    "content": "\"linked_account_id\",\"company\",\"division\",\"department\",\"region\"\n\"222222222\",\"Company A\",\"Technology\",\"R&D\",\"US\"\n\"1111111111\",\"Company A\",\"Sales\",\"Marketing\",\"EU\"\n\"33333333333\",\"Company A\",\"Finance\",\"Budget\",\"EU\""
    }
}'
```

Provide the CSV content as a string.

> Response Example:

```json
{
    "id" : "1234",
    "uploadDate" : "2022-02-02 10:10"
}
```
