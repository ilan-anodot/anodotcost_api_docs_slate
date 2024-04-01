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

<aside class="warning">
This call is being deprecated.</br>
The call is being replaced by the v2 call described below.</br>
</aside>

### Get cost forecast data V2

> Request Example: GET cost forecast V2

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/dashboards/cost-forecast-data-v2' \
--header 'apikey: {{account-api-key}}' \
--header 'authorization: {{Bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}'
```

**Summary:** Retrieves cost forecast data from your main dashboard.


> Response example: Cost forecast data

```json
{
[
    {
        "prevMonth": "34778.306663999996",
        "day": "01",
        "fullDate": "2023-06-01T00:00:00.000Z"
    },
    {
        "prevMonth": "2683.4397510000003",
        "day": "02",
        "fullDate": "2023-06-02T00:00:00.000Z"
    },
    {
        "prevMonth": "2656.5000919999998",
        "day": "03",
        "fullDate": "2023-06-03T00:00:00.000Z"
    },
    {
        "prevMonth": "2515.5461549999986",
        "day": "04",
        "fullDate": "2023-06-04T00:00:00.000Z"
    },
    {
        "prevMonth": "1999.647648",
        "day": "05",
        "fullDate": "2023-06-05T00:00:00.000Z"
    },
    {
        "prevMonth": "2294.1456880000005",
        "day": "06",
        "fullDate": "2023-06-06T00:00:00.000Z"
    },
    {
        "prevMonth": "2194.124421",
        "day": "07",
        "fullDate": "2023-06-07T00:00:00.000Z"
    },
    {
        "prevMonth": "2404.0058640000007",
        "day": "08",
        "fullDate": "2023-06-08T00:00:00.000Z"
    },
    {
        "prevMonth": "1744.162223",
        "day": "09",
        "fullDate": "2023-06-09T00:00:00.000Z"
    },
    {
        "prevMonth": "2116.258666",
        "day": "10",
        "fullDate": "2023-06-10T00:00:00.000Z"
    },
    {
        "prevMonth": "2270.999953",
        "day": "11",
        "fullDate": "2023-06-11T00:00:00.000Z"
    },
    {
        "prevMonth": "2004.3149509999998",
        "day": "12",
        "fullDate": "2023-06-12T00:00:00.000Z"
    },
    {
        "prevMonth": "2849.893763",
        "day": "13",
        "fullDate": "2023-06-13T00:00:00.000Z"
    },
    {
        "prevMonth": "3010.746488",
        "day": "14",
        "fullDate": "2023-06-14T00:00:00.000Z"
    },
    {
        "prevMonth": "2811.8823840000005",
        "day": "15",
        "fullDate": "2023-06-15T00:00:00.000Z"
    },
    {
        "prevMonth": "3446.400334",
        "day": "16",
        "fullDate": "2023-06-16T00:00:00.000Z"
    },
    {
        "prevMonth": "3223.859170999999",
        "day": "17",
        "fullDate": "2023-06-17T00:00:00.000Z"
    },
    {
        "prevMonth": "3307.9344219999994",
        "day": "18",
        "fullDate": "2023-06-18T00:00:00.000Z"
    },
    {
        "prevMonth": "3097.7618540000008",
        "day": "19",
        "fullDate": "2023-06-19T00:00:00.000Z"
    },
    {
        "prevMonth": "2972.9151269999998",
        "day": "20",
        "fullDate": "2023-06-20T00:00:00.000Z"
    },
    {
        "prevMonth": "3530.1560849999996",
        "day": "21",
        "fullDate": "2023-06-21T00:00:00.000Z"
    },
    {
        "prevMonth": "3371.194889",
        "day": "22",
        "fullDate": "2023-06-22T00:00:00.000Z"
    },
    {
        "prevMonth": "3849.7330609999995",
        "day": "23",
        "fullDate": "2023-06-23T00:00:00.000Z"
    },
    {
        "prevMonth": "3521.5956419999998",
        "day": "24",
        "fullDate": "2023-06-24T00:00:00.000Z"
    },
    {
        "prevMonth": "3913.8737509999996",
        "day": "25",
        "fullDate": "2023-06-25T00:00:00.000Z"
    },
    {
        "prevMonth": "3522.613269",
        "day": "26",
        "fullDate": "2023-06-26T00:00:00.000Z"
    },
    {
        "prevMonth": "4091.5025979999996",
        "day": "27",
        "fullDate": "2023-06-27T00:00:00.000Z"
    },
    {
        "prevMonth": "3725.599599",
        "day": "28",
        "fullDate": "2023-06-28T00:00:00.000Z"
    },
    {
        "prevMonth": "3077.688439",
        "day": "29",
        "fullDate": "2023-06-29T00:00:00.000Z"
    },
    {
        "prevMonth": "3787.2526660000003",
        "currMonth": "3787.2526660000003",
        "day": "30",
        "fullDate": "2023-06-30T00:00:00.000Z"
    },
    {
        "currMonth": "25242.344936",
        "day": "01",
        "fullDate": "2023-07-01T00:00:00.000Z"
    },
    {
        "currMonth": "4098.989780000001",
        "day": "02",
        "fullDate": "2023-07-02T00:00:00.000Z"
    },
    {
        "currMonth": "3615.3786949999985",
        "day": "03",
        "fullDate": "2023-07-03T00:00:00.000Z"
    },
    {
        "currMonth": "3066.5577670000002",
        "day": "04",
        "fullDate": "2023-07-04T00:00:00.000Z"
    },
    {
        "currMonth": "3150.851217000001",
        "day": "05",
        "fullDate": "2023-07-05T00:00:00.000Z"
    },
    {
        "currMonth": "3261.073089",
        "day": "06",
        "fullDate": "2023-07-06T00:00:00.000Z"
    },
    {
        "currMonth": "3339.8500399999994",
        "day": "07",
        "fullDate": "2023-07-07T00:00:00.000Z"
    },
    {
        "currMonth": "3749.585996",
        "day": "08",
        "fullDate": "2023-07-08T00:00:00.000Z"
    },
    {
        "currMonth": "3241.640188",
        "day": "09",
        "fullDate": "2023-07-09T00:00:00.000Z"
    },
    {
        "currMonth": "3145.589774",
        "currExpected": "3145.589774",
        "day": "10",
        "fullDate": "2023-07-10T00:00:00.000Z"
    },
    {
        "currExpected": "3116.590501845878",
        "day": "11",
        "fullDate": "2023-07-11T00:00:00.000Z"
    },
    {
        "currExpected": "3087.5909616494705",
        "day": "12",
        "fullDate": "2023-07-12T00:00:00.000Z"
    },
    {
        "currExpected": "3058.5913794103667",
        "day": "13",
        "fullDate": "2023-07-13T00:00:00.000Z"
    },
    {
        "currExpected": "3029.59175512832",
        "day": "14",
        "fullDate": "2023-07-14T00:00:00.000Z"
    },
    {
        "currExpected": "3000.5920888032506",
        "day": "15",
        "fullDate": "2023-07-15T00:00:00.000Z"
    },
    {
        "currExpected": "2971.5923804352433",
        "day": "16",
        "fullDate": "2023-07-16T00:00:00.000Z"
    },
    {
        "currExpected": "2942.592731194434",
        "day": "17",
        "fullDate": "2023-07-17T00:00:00.000Z"
    },
    {
        "currExpected": "2913.593039910648",
        "day": "18",
        "fullDate": "2023-07-18T00:00:00.000Z"
    },
    {
        "currExpected": "2884.5933065839486",
        "day": "19",
        "fullDate": "2023-07-19T00:00:00.000Z"
    },
    {
        "currExpected": "2855.5935312145643",
        "day": "20",
        "fullDate": "2023-07-20T00:00:00.000Z"
    },
    {
        "currExpected": "2826.59371380289",
        "day": "21",
        "fullDate": "2023-07-21T00:00:00.000Z"
    },
    {
        "currExpected": "2797.5938543494854",
        "day": "22",
        "fullDate": "2023-07-22T00:00:00.000Z"
    },
    {
        "currExpected": "2768.593952855077",
        "day": "23",
        "fullDate": "2023-07-23T00:00:00.000Z"
    },
    {
        "currExpected": "2739.5941104887906",
        "day": "24",
        "fullDate": "2023-07-24T00:00:00.000Z"
    },
    {
        "currExpected": "2710.5942260813886",
        "day": "25",
        "fullDate": "2023-07-25T00:00:00.000Z"
    },
    {
        "currExpected": "2681.5942996335743",
        "day": "26",
        "fullDate": "2023-07-26T00:00:00.000Z"
    },
    {
        "currExpected": "2652.594331146217",
        "day": "27",
        "fullDate": "2023-07-27T00:00:00.000Z"
    },
    {
        "currExpected": "2623.5943206203515",
        "day": "28",
        "fullDate": "2023-07-28T00:00:00.000Z"
    },
    {
        "currExpected": "2594.594268057178",
        "day": "29",
        "fullDate": "2023-07-29T00:00:00.000Z"
    },
    {
        "currExpected": "2565.594173458063",
        "day": "30",
        "fullDate": "2023-07-30T00:00:00.000Z"
    },
    {
        "currExpected": "2536.594137987867",
        "day": "31",
        "fullDate": "2023-07-31T00:00:00.000Z"
    },
    {
        "currExpected": "2507.594917019923",
        "day": "01",
        "fullDate": "2023-08-01T00:00:00.000Z"
    },
    {
        "currExpected": "2478.5956540160855",
        "day": "02",
        "fullDate": "2023-08-02T00:00:00.000Z"
    },
    {
        "currExpected": "2449.5963489750266",
        "day": "03",
        "fullDate": "2023-08-03T00:00:00.000Z"
    },
    {
        "currExpected": "2420.5970018955836",
        "day": "04",
        "fullDate": "2023-08-04T00:00:00.000Z"
    },
    {
        "currExpected": "2391.59761277676",
        "day": "05",
        "fullDate": "2023-08-05T00:00:00.000Z"
    },
    {
        "currExpected": "2362.598181617724",
        "day": "06",
        "fullDate": "2023-08-06T00:00:00.000Z"
    },
    {
        "currExpected": "2333.598809584317",
        "day": "07",
        "fullDate": "2023-08-07T00:00:00.000Z"
    },
    {
        "currExpected": "2304.599395510766",
        "day": "08",
        "fullDate": "2023-08-08T00:00:00.000Z"
    },
    {
        "currExpected": "2275.5999393962165",
        "day": "09",
        "fullDate": "2023-08-09T00:00:00.000Z"
    },
    {
        "currExpected": "2246.600441239981",
        "day": "10",
        "fullDate": "2023-08-10T00:00:00.000Z"
    }
]
}
```

**Response**

| field | Description |
| ---- | ----------- |
| day | day of the month, values 01-31 |
| fullDate | Full date, including the year, month, day |
| *Value type* | *One or more of the types below:* |
| prevMonth | Actual values from the previous month(s) |
| currMonth | Actual values from the current month |
| currExpected | Forecasted values for the current month and future months |


<aside class="notice">
Days in the result may include more than one type of values.</br>
This is due to transition from previous to current month, from actual data to forecasted data.</br>
When processing the results further, make sure you address these special days.</br>
</aside>