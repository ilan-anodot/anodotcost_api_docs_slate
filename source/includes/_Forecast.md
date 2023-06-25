## Cost Forecast
### Get cost forecast data

> Request Example: GET cost forecast

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/dashboards/cost-forecast-data' \
--header 'apikey: {{account-api-key}}' \
--header 'authorization: {{Bearer-token}}'
```

**Summary:** Retrieves cost forecast data from your main dashboard

> Response example: Cost forecast data

```json
{
    [
    {
        "date": "1",
        "prevMonth": 36697.19,
        "currMonth": 17727
    },
    {
        "date": "2",
        "prevMonth": 2839.32,
        "currMonth": 2847.21
    },
    {
        "date": "3",
        "prevMonth": 2966.28,
        "currMonth": 2766.56
    },
    {
        "date": "4",
        "prevMonth": 3590.78,
        "currMonth": 2725.13
    },
    {
        "date": "5",
        "prevMonth": 3421.61,
        "currMonth": 2648.63
    },
    {
        "date": "6",
        "prevMonth": 3355.73,
        "currMonth": 2655.76,
        "currExpected": 2655.76
    },
    {
        "date": "7",
        "prevMonth": 3264.97,
        "currExpected": 2675.7610976458977
    },
    {
        "date": "8",
        "prevMonth": 3206.11,
        "currExpected": 2675.7610516705236
    },
    {
        "date": "9",
        "prevMonth": 3025.87,
        "currExpected": 2675.7610056938606
    },
    {
        "date": "10",
        "prevMonth": 3055.86,
        "currExpected": 2675.7609597160904
    },
    {
        "date": "11",
        "prevMonth": 2794.65,
        "currExpected": 2675.7609137373943
    },
    {
        "date": "12",
        "prevMonth": 2946.73,
        "currExpected": 2675.7608677579537
    },
    {
        "date": "13",
        "prevMonth": 2710.63,
        "currExpected": 2675.760762002647
    },
    {
        "date": "14",
        "prevMonth": 2865.67,
        "currExpected": 2675.760716023484
    },
    {
        "date": "15",
        "prevMonth": 2741.55,
        "currExpected": 2675.7606700437755
    },
    {
        "date": "16",
        "prevMonth": 2658.69,
        "currExpected": 2675.7606240637024
    },
    {
        "date": "17",
        "prevMonth": 2952.45,
        "currExpected": 2675.760578083447
    },
    {
        "date": "18",
        "prevMonth": 2742.47,
        "currExpected": 2675.7605321031892
    },
    {
        "date": "19",
        "prevMonth": 2788.93,
        "currExpected": 2675.760486123111
    },
    {
        "date": "20",
        "prevMonth": 2910.67,
        "currExpected": 2675.76038037177
    },
    {
        "date": "21",
        "prevMonth": 2863.9,
        "currExpected": 2675.7603343923383
    },
    {
        "date": "22",
        "prevMonth": 2900.4,
        "currExpected": 2675.7602884132853
    },
    {
        "date": "23",
        "prevMonth": 2958.82,
        "currExpected": 2675.760242434792
    },
    {
        "date": "24",
        "prevMonth": 3050.14,
        "currExpected": 2675.76019645704
    },
    {
        "date": "25",
        "prevMonth": 3023.89,
        "currExpected": 2675.7601504802105
    },
    {
        "date": "26",
        "prevMonth": 3007.59,
        "currExpected": 2675.760104504485
    },
    {
        "date": "27",
        "prevMonth": 2930.77,
        "currExpected": 2675.7599987640224
    },
    {
        "date": "28",
        "prevMonth": 2909.82,
        "currExpected": 2675.759952789311
    }
]
}
```

**Response**

The response will be the forecasted cost for every day in the current month.
You will get entries from 1 to 31 (days of the month) with:

* The cost value of the previous month
* The cost value of the current month
    * If the day has already passed - the actual value
    * If the day is in the future - the forecasted value for it
