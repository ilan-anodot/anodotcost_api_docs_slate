# Tags
A tag is a property/value pair that can be dynamically added/removed to a metric or set of metrics without effecting the name of the metric.
For example:  a tag can be accountManager=Joe which will be attached to the metrics related to the accounts Joe manages. If the accounts Joe manages change, the API lets you change the assignment of the tags to metrics without affecting the metric name.
Tags can be used to filter and aggregate data.
<aside class="notice">
1. If a tag already exists then the tag will be removed from metrics that previously matched the search expression. The tag will be added only to the metrics that match the current search expression.
2. Tags MUST NOT contain the following characters:{ . SPACE}
3. It can take up to 20 minutes till the metric tagging operation will be applied to the relevant metrics.
</aside>

## Create Tags

### Description
To create a tag on a set of metrics that match a search expression.
Multiple expressions can be associated to each tag which is treated an OR expression between them.

> POST  https://api.anodot.com/api/v1/metrics/tags?token=<api token>

### Definition

```json
 Content-type=application/json
 ```
### Headers

### Body
```json
{
"tag": {
    "key": "tagName",
  "value": "tagValue"
  },
  "expressions": [
    {
      "expression": [
        {
          "type": "property",
          "key": "key1",
          "value": "val1"
      },
      {
          "type": "property",
          "key": "key2",
          "value": "val2"
      }
     ]
    },
    {
      "expression": [
        {
          "type": "property",
          "key": "key1",
          "value": "val3"
        },
        {
          "type": "property",
          "key": "key2",
          "value": "val4"
        }
      ]
    }
  ]
}
```
### Arguments
Argument | Definition
---------|-----------
tag | key: tag key name
| value: tag key value
expressions |  Array of expressions, each expression can contain several metrics queries.
| type: the type of the search expression, can be of type [*search*] or [*property*] (recommended).
| key: the metric key name
| value: the metric key value 
| in the above example all metrics return from the query:
| ((key1 == val1) AND (key2 == val2)) OR ((key1 ==val3) AND (key2 ==val4)) 
| will be tagged with the tag: tagName:tagValue

### Example Request
```shell
curl \
-X POST \
-d '{"tag":{"key":"my_campaigns","value":"12345678ABC"},"expressions":[{"expression":[{"type":"property","key":"what","value":"impressions"},{"type":"property","key":"campaign_id","value":"12345678ABC"}]}]}' \
-H "Content-Type: application/json" \
'https://api.anodot.com/api/v1/metrics/tags?token=<api token>'
```

### Example Response
```json
{
   "validation":{
      "passed":true,
      "failures":[

      ]
   },
   "tags":[
      {
         "id":"8b6f6fa8-ba46-4596-8358-ebcb4727f5ee",
         "expressions":[
            {
               "expression":[
                  {
                     "type":"property",
                     "key":"what",
                     "value":"impressions"
                  },
                  {
                     "type":"property",
                     "key":"campaign_id",
                     "value":"12345678ABC"
                  }
               ]
            }
         ],
         "tag":{
            "key":"my_campaigns",
            "value":"12345678ABC"
         }
      }
   ]
}
```
<aside=="notice">
For more details about example responses, see HTTP Responses. 
</aside>

## Get Tags

### Description
Returns a tag's configuration based on the tag's ID.

### Definition
> GET https://api.anodot.com/api/v1/metrics/tags/:Id?token=<api token>

### Headers
```json
 Content-type=application/json
 ```

### Arguments
Argument | Definition
-------- | ----------
ID | The ID of the category

### Example Request
```shell
curl \
-X GET \
-H "Content-Type: application/json" \
'https://api.anodot.com/api/v1/metrics/tags/31425ce5-32c1-4cce-9075-28c1748d42a8?token=<api token>'
```

### Example Response
```json
[
   {
      "id":"31425ce5-32c1-4cce-9075-28c1748d42a8",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470056853,
      "disabled":false
   },
   {
      "id":"8b6f6fa8-ba46-4596-8358-ebcb4727f5ee",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470058877,
      "disabled":false
   },
   {
      "id":"a56259ce-38df-4f0a-87a9-157198c82275",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470058253,
      "disabled":false
   },
   {
      "id":"f9c4bec4-632d-4366-b78d-65323fbc4e84",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470058449,
      "disabled":false
   }
]
```

<aside="notice">
For more details about example responses, see HTTP Responses.
</aside>

## List All tags

### Description
List all tag configurations in the account sent via the tag APIs.

### Defintion
> GET https://api.anodot.com/api/v1/metrics/tags?token=<api token>

### Headers
```json
Content-type=application/json
```

### Example Request
```shell
curl \
-X GET \
-H "Content-Type: application/json" \
'https://api.anodot.com/api/v1/metrics/tags?token=<api token>'
```

### Example Responses
```json
[
   {
      "id":"31425ce5-32c1-4cce-9075-28c1748d42a8",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470056853,
      "disabled":false
   },
   {
      "id":"8b6f6fa8-ba46-4596-8358-ebcb4727f5ee",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470058877,
      "disabled":false
   },
   {
      "id":"a56259ce-38df-4f0a-87a9-157198c82275",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470058253,
      "disabled":false
   },
   {
      "id":"f9c4bec4-632d-4366-b78d-65323fbc4e84",
      "expressions":[
         {
            "expression":[
               {
                  "type":"property",
                  "key":"what",
                  "value":"impressions"
               },
               {
                  "type":"property",
                  "key":"campaign_id",
                  "value":"12345678ABC"
               }
            ]
         }
      ],
      "tag":{
         "key":"my_campaigns",
         "value":"12345678ABC"
      },
      "lastModified":1470058449,
      "disabled":false
   }
]
```
<aside="notice">
 For more details about example responses, see HTTP Responses.
</aside>

## Delete Tags

### Description
Delete a tag and remove it from all metrics.

### Definition
> DELETE https://api.anodot.com/api/v1/metrics/tags/:id?token=<api token>

### Arguments
Argument | Definition
-------- | ----------
ID | The ID of the category

### Example Request
```shell
curl \
-X DELETE \
-H "Content-Type: application/json" \
'https://apiExample.anodot.com/api/v1/metrics/tags/31425ce5-32c1-4cce-9075-28c1748d42a8?token=<api token>'
```

### Example Response
No response body for this API
