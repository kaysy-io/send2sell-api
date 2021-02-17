# Retreive all the states and their codes

In many places throughout the application two letter state code will be required for processing. For example, order billing details requires a two letter state code. These state code can be fetched using the endpoint given below.

|                |                                      |
| -------------- | ------------------------------------ |
| Method         | **GET**                              |
| Endpoint       | https://www.send2sell.com/api/states |
| Authentication | **Not required**                     |

## Success response

A sample success response is given below

```json
[
  {
    "id": 1,
    "code": "AL",
    "name": "Alabama",
    "tax_rate": 0
  },
  {
    "id": 2,
    "code": "AK",
    "name": "Alaska",
    "tax_rate": 0
  },
  /**
   * ..
   * ..
   * Data of all the states
   * ..
   * ..
   */
  {
    "id": 53,
    "code": "AE",
    "name": "Armed Forces (AE)",
    "tax_rate": 0
  },
  {
    "id": 54,
    "code": "AP",
    "name": "Armed Forces (AP)",
    "tax_rate": 0
  }
]
```

The parameters are self explanatory. The `code` field value is what has to be used in places where a `state` field is required.
