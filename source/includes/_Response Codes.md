## Response Codes

### HTTP Responses
Anodot APIs use HTTP status codes to indicate the success or failure of a request.

An error indicates that the service did not successfully handle your request. In addition to the status code, the response may contain a JSON object with an errors array containing more detailed error messages (see Error response format bellow).

If the service is able to handle your request, but some issues are present (e.g. one of the metric sent has an illegal timestamp but all the reset are ok), the HTTP status code will indicate success and the response body will contain the expected result with the addition of errors array containing detailed error messages.

### HTTP Status Codes

Error Code | Meaning
-|-
200 | OK
400 | Bad Request
401 | Unauthorized
403 | Forbidden
404 | Not Found
500 | Server error

### Error Response Fields

> Error Response Example

```json
{ 
  “errors”:[ 
    {
      "index":"failed sample index",
      “error”: "<error code>", 
      “description”:”error description” 
    } 
  ] 
}
```

Field | Description
------|------------
errors | An array of errors received from the request
index | In case a batch of records was sent in the request, the index directs to the faulty entry
error | Error code
description | The error Description

The [error code list](#anodot-error-codes)


