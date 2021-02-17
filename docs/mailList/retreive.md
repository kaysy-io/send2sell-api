# Retreive all mail list details

Flyer creation requires a valid `mail_list_id` field which accepts only the `id` of a Send2Sell email list. So, to create a flyer using Send2Sell API, it is necessary to know the `id` of all the Send2Sell mail lists. This doc will help you fetch the mail list details from Send2Sell.

|                |                                                |
| -------------- | ---------------------------------------------- |
| Method         | **GET**                                        |
| Endpoint       | https://www.send2sell.com/api/states/mailLists |
| Authentication | **Not required**                               |

## Success response

This endpoint will return all the active mail lists of Send2Sell grouped by states. A sample response is shown below.

```json
{
  "AZ": {
    "id": 3,
    "code": "AZ",
    "name": "Arizona",
    "lists": [
      {
        "id": 18,
        "state_id": 3,
        "name": "Maricopa County",
        "active": 1
      },
      {
        "id": 42,
        "state_id": 3,
        "name": "Pima County",
        "active": 1
      }
    ]
  },
  /**
   * ..
   * ..
   * Other states and list details
   * ..
   * ..
   */
  "VA": {
    "id": 47,
    "code": "VA",
    "name": "Virginia",
    "lists": [
      {
        "id": 49,
        "state_id": 47,
        "name": "Fairfax County",
        "active": 1
      }
    ]
  }
}
```

The response JSON object is a collection of states with state code mapped to the state details. This state details object contains the data of all the available mail lists for that state.

In the above example, value of key `AZ` is the details of `Arizona`. The `lists` array contains all the mail list of Arizona. The sample shows 2 lists for Arizona - Maricopa County and Pima county list.

As you can interpret, `id` of Maricopa County is **18** and this is the value to be used for the `mail_list_id` field, if the flyer has to be delivered to Maricopa County agents.

Similarly for Pima County, it is 42 and for Fairfax County, it is 49.
