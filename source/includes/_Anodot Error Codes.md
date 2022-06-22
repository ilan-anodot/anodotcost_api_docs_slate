## Anodot Error Codes

Error Code | Description
---------- | -----------
1001 | Json parsing error
1002 | General error
1004 | API calls rate exceeded the maximum allowed
1005 | Number of streams exceeded the maximum allowed
1006 | Number of incomplete streams exceeded the maximum allowed
1007 | Number of Data Sources exceeded the maximum allowed
2001 | Token is not active
2002 | Too many concurrent requests
2003 | Metrics per seconds limit reached
2004 | Metric properties are not defined. Must have at least one property.
2005 | Metric properties limit exceeded. Metric may contain up to: {xxx} properties.
2006 | Undefined property key. Metric property key must contain at least 1 character.
2007 | Property: ''{xxx}'' key length exceeded. maximum key length is {yyy} characters.
2008 | Undefined property value. Metric property value must contain at least 1 character.
2009 | Property: ''{xxx}'' value length exceeded. Maximum value length is {yyy} characters .
2010 | Property key: ''{xxx}'' contains illegal characters.  ''.'' character and space characters are not allowed.
2011 | Property value: ''{xxx}'' contains illegal characters. ''.'' character and space characters are not allowed.
2012 | Property: ''what'' is undefined. ''what'' is a mandatory property that specified what is being measured by this metric.
2013 | Metric tags limit exceeded. Metric may contain up to: {xxx} tags.
2014 | Undefined tag key. Metric tag key must contain at least 1 character.
2015 | Tag: ''{xxx}'' key length exceeded. Maximum key length is {yyy} characters.
2016 | Undefined tag value. Metric tag value must contain at least 1 character.
2017 | Tag: ''{xxx}'' value length exceeded. Maximum value length is {yyy} characters.
2018 | Tag key: ''{xxx}'' contains illegal characters. ''.'' character and space characters are not allowed.
2019 | Tag value: ''{xxx}'' contains illegal characters. ''.'' character and space characters are not allowed.
2020 | Metric timestamp is undefined. Timestamp must be an epoch time in seconds.
2021 | Metric timestamp: ''{xxx}'' is not a valid epoch time. Timestamp must be an epoch time in seconds.
2022 | Metric timestamp: ''{xxx}'' is in the future. Timestamp must be an epoch time in seconds.
2023 | Metric timestamp: ''{xxx}'' is negative. Timestamp must be an epoch time in seconds.
2024 | Metric value is undefined. The ''value'' must be a valid decimal double precision number.
2025 | Metric value: ''{xxx}'' is not a valid decimal number.  The ''value'' must be a valid decimal double precision number.
2026 | Metric value is NaN or Infinite
2027 | Metric name is null or empty
2028 | Metric name length exceeded the maximum allowed: {xxx}.
2029 | Invalid protocol - supported protocols are: {xxx}. 
2030 | Invalid target type: ''{xxx}''. valid values are: {yyy}.

<aside class="notice">
The list is partial. Response codes are added for new and existing endpoints.
</aside>
