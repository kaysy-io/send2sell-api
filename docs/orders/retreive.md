# Retreive an order

An authenticated user can retrieve the details of an order using the id, if the user has access to it.

|                |                                               |
| -------------- | --------------------------------------------- |
| Method         | **GET**                                       |
| Endpoint       | https://www.send2sell.com/api/order/:id       |
| Parameter      | The id of the order that has to be retreived. |
| Authentication | **Required**                                  |

Say, you want to retrieve the details of order **#356972**, all you have to do is to make a GET request to the url `https://www.send2sell.com/api/order/356972` along with the access token.

An example for the same in PHP is given below

```php
<?php

use GuzzleHttp\Client;

$response = (new Client())->get('https://www.send2sell.com/api/order/356972', [
    // Replace `YOUR_ACCESS_TOKEN` with the token you received
    // when performing authentication.
    'headers'  => ['Authorization' => 'Bearer YOUR_ACCESS_TOKEN']
]);
$data = $response->getBody();

// Process the data as per your use case.
```

### Success response

If the requesting user has read access to the order, a JSON response will be returned. A sample response is shown below.

```json
{
  "id": 356972,
  "paypal": 0,
  "status": "Scheduled",
  "created_at": "2020-11-04T14:27:31-06:00",
  "city": "West Hollywood",
  "cost": 69,
  "discount": 0,
  "first_name": "Owen",
  "last_name": "Salkin",
  "phone": "310-850-0283",
  "state": "CA",
  "street": "8560 Sunset Blvd",
  "tax": 0,
  "total": 69,
  "zip": "90069",
  "payment_method": "Card ending in 0504",
  "credits_consumed": 0,
  "credits_needed": 69,
  "net_total": 69,
  "products": {
    "items": 1,
    "flyers": [
      {
        "id": 376912,
        "status": "Scheduled",
        "delivered": 0,
        "opened": 4,
        "clicked": 0,
        "prop_address": "1730 Sawtelle Blvd #112 Los Angeles, CA 90025",
        "prop_desc": "<p>If you've seen this before, wait until you see it now! There is a quintessential townhouse dubbed \"Japantown\" just waiting for you. If you love all that Sawtelle has to offer, then this beautiful 2 beds (including a mounted T.V.)  2 baths townhouse is just for you. The unit has soaring ceilings, contemporary lighting, no common walls and both travertine floors and gray hardwood floors. This modern building, completed in 2010, has a lovely outdoor seating area with fireplace, side by side parking in a secured parking garage, a storage room, plenty of guest parking and bicycle racks. The kitchen includes all high end Energy Star stainless appliances, Caesar-stone counters and washer & dryer.  There is pre-wired internet and WiFi, w/ an extra fast connection included in the HOA fee. Owner can choose between cable or satellite. Building has 24-hour surveillance cameras, HOA includes water, gas and Earthquake insurance. Adjacent to trendy restaurants, Santa Monica, UCLA, Westwood.</p>",
        "prop_price": "$835,500",
        "main_image": "/uploads/1604521137-Front.jpg",
        "headline": "Gorgeous Townhouse in newer building",
        "sub_headline": "Ready to move into",
        "cost": 69,
        "rush": "0",
        "scheduled": "2020-11-06T14:23:00-06:00",
        "subject": "Townhouse in West Los Angeles, great value",
        "list_name": "LA County, CA",
        "percentage_completed": 1
      }
    ]
  },
  "coupon_name": "",
  "invoice_url": "http://127.0.0.1:8000/order/370562/invoice?signature=61a645cb1070aa3744775999c428c7d762befe2e101675ad596731dd7aa7e996"
}
```

If the requesting user has no access to the requested order, then an HTTP `403` error response will be returned

```json
{
  "message": "You are not authorized to access this order."
}
```

## Response field details

#### _id_

|           |             |
| :-------- | :---------- |
| Data type | **Integer** |

The unique id of the order.

#### _paypal_

|           |             |
| :-------- | :---------- |
| Data type | **Integer** |

This will be either 0 or 1. The value indicates whether the payment is carried out through PayPal or not.

#### _status_

|           |            |
| :-------- | :--------- |
| Data type | **String** |

The current status of the order. An order will always be in either one of these status' - `Payment Pending`, `Partially Paid`, `Paid`, `Cancelled`, `Completed`, and `Refunded`.

#### _created_at_

|           |            |
| :-------- | :--------- |
| Data type | **String** |

The order creation time in CST/CDT.

#### _cost_

|           |            |
| :-------- | :--------- |
| Data type | **Number** |

The total cost of all the products ordered.

#### _discount_

|           |            |
| :-------- | :--------- |
| Data type | **Number** |

The total applied discount amount.

#### _tax_

|           |            |
| :-------- | :--------- |
| Data type | **Number** |

Sales tax collected on the order.

#### _total_

|           |            |
| :-------- | :--------- |
| Data type | **Number** |

The total amount that has to be paid for the order after any discounts or prepaid credit consumptions.

#### _credits_consumed_

|           |                    |
| :-------- | :----------------- |
| Data type | **Number or NULL** |

The total credits consumed by Send2Sell for this order. This field will be `undefined` or `null` if no credits were consumed yet.

#### _credits_needed_

|           |            |
| :-------- | :--------- |
| Data type | **Number** |

The total credits needed for marking the order as `Paid`.

`credits_needed = cost - discount - credits_consumed`

#### _net_total_

|           |            |
| :-------- | :--------- |
| Data type | **Number** |

This is the total amount collected by Send2Sell for this order after any refunds.

#### _payment_method_

|           |            |
| :-------- | :--------- |
| Data type | **String** |

The payment method used for this order. Send2Sell supports four kinds of payments and the value will be either one of `Promotional Code`, `Prepaid Credits`, `PayPal`, and `Card ending in XXXX`.

#### _first_name_

|           |            |
| :-------- | :--------- |
| Data type | **String** |

First name of the order billing details.

#### _last_name_

|           |            |
| :-------- | :--------- |
| Data type | **String** |

Last name of the order billing details.

#### _street_

|           |            |
| :-------- | :--------- |
| Data type | **String** |

Street address of the order billing details.

#### _city_

|           |            |
| :-------- | :--------- |
| Data type | **String** |

City name of the order billing details.

#### _state_

|           |            |
| :-------- | :--------- |
| Data type | **String** |

Two letter state code of the order billing details.

#### _zip_

|           |            |
| :-------- | :--------- |
| Data type | **String** |

Zip code of the order billing details.

#### _phone_

|           |            |
| :-------- | :--------- |
| Data type | **String** |

Contact number of the billing user.

#### _coupon_name_

|           |            |
| :-------- | :--------- |
| Data type | **String** |

Name of the coupon applied on the order.

#### _invoice_url_

|           |            |
| :-------- | :--------- |
| Data type | **String** |

The absolute URL of the order invoice.

#### _products_

|           |                                       |
| :-------- | :------------------------------------ |
| Data type | [**Object**](#products-object-fields) |

Detailed summary of all the products in this order.

### Products Object Fields

An order products object contains all the products linked to this order and an `items` field which gives the count of total products in this order.

In the sample response given above, you can see products object containing a single flyer item. So the `items` field shows `1` as value and the other field is `flyers` which is an array of all the flyers in this order.

> At the moment Send2Sell sells only e-flyer campaigns, so `flyers` and `items` will be the only two keys present in the products object. In future we will be adding `addOns`. That will again be an array containing the summary of individual addOn items.

The `flyers` key maps to an array of flyer items. Refer the flyer docs for the details of each flyer fields.

```json
{
  "products": {
    "items": 2,
    "flyers": [
      {
        "id": 375680,
        "status": "Scheduled",
        "delivered": 0,
        "opened": 0,
        "clicked": 0
        /**
         * Other flyer fields
         */
      },
      {
        "id": 375681,
        "status": "Completed",
        "delivered": 23520,
        "opened": 9876,
        "clicked": 1234
        /**
         * Other flyer fields
         */
      }
    ]
  }
  /**
   * Other order fields
   */
}
```
