# Ordering a flyer

A user can order a flyer using the Send2Sell API only if they have enough prepaid credit balance. Otherwise, the order has to be paid from the Send2Sell web application itself.

Ordering a flyer is a two step process.

1. Create an order for the flyer, which is explained in this guide.
2. Make payment of the order using the `id` obtained in the above step. Refer the [order payment](https://github.com/kaysy-io/send2sell-api/blob/main/docs/authentication/newAccessToken.md) doc for the implementation details.

Once the payment is successfull, the flyer will be automatically scheduled for delivery. The same can be verified from the payment success response or by fetching the flyer details again.

|                |                                           |
| -------------- | ----------------------------------------- |
| Method         | **POST**                                  |
| Endpoint       | https://www.send2sell.com/api/flyer/order |
| Authentication | **Required**                              |

## Quick order Request

To create an order of a flyer, the whole flyer details has to be submitted to the given API endpoint. An example for the same in PHP is given below.

```php
<?php

use GuzzleHttp\Client;

$response = (new Client())->get('https://www.send2sell.com/api/flyer/order', [
    // Replace `YOUR_ACCESS_TOKEN` with the token you received
    // when performing authentication.
    'headers'  => ['Authorization' => 'Bearer YOUR_ACCESS_TOKEN'],
    'form_params' => [
        "campaigns" => [
            [
                "mail_list_id" => 8,
                "subject" => "Open house | 59 Westfield Avenue Daly City",
                "rush" => 1,
                "scheduled" => "2021-02-17T05:46:39-06:00",
                "id" => 376918
            ]
        ],
        "name" => "Rebecca White",
        "license" => null,
        "company" => "Intero Real Estate Services",
        "address_1" => "2488 Junipero Serra Blvd",
        "address_2" => "Daly City, CA 94015",
        "email" => "RebeccaW@Intero.com",
        "phone" => "4154121977",
        "headshot" => null,
        "logo" => null,
        "prop_address" => "59 Westfield Avenue Daly City, CA 94015",
        "prop_desc" => "<p>Gorgeous remodeled Westlake Highlands home with 4 bedrooms and 3 bathrooms.Immaculate move-in condition! Open kitchen with beautiful cherry kitchen cabinets, stainless appliances and granite counters. Hardwood floors, crown molding and recessed lights throughout. Upper/main level has 2 bedrooms and 1 bathroom. Downstairs has 2 bedrooms and 2 bathrooms plus a family/living room. Laundry appliances in garage.Fabulous 3-level, terraced backyard full of lovely flowers, apple trees and lemon trees that produce fruit year 'round. Newly added garden shed for extra storage. Close to Westlake for all of your shopping and dining pleasure, Lake Merced Golf Course, Westmoor High School, Mussel Rock &amp; Longview Parks, Civic Center, and Great Highway. Virtual Open Houses: Saturday, November 7, 1-4 pm and Sunday, November 1 and 8, 1-4 pm&nbsp;<br>Calendly link<br>Zoom link: &nbsp;<a href=\"https://zoom.us/j/97736601984\">https://zoom.us/j/97736601984</a><br>&nbsp;</p>",
        "prop_price" => "$1,200,000",
        "prop_details" => "4 bedrooms, 3 full bathrooms, 1610 square feet, attached garage, yard,",
        "main_image" => "/downloads/1604534765-39628710fb4020bf76c51c72e4d4ce31-full.jpg",
        "custom" => "0",
        "theme_color" => "#444444",
        "theme_font_color" => "#FFFFFF",
        "accent_color" => "#7D0F0F",
        "headline" => "Virtual Open Houses/Showings by Appointment Saturday & Sunday 1 pm-4 pm",
        "sub_headline" => "4 Bed, 3 Bath Westlake Highlands Home with Bonus Rooms and FABULOUS Yard!",
        "template" => "template_2",
        "additional_images" => [
            "image_0" => "",
            "image_1" => "",
            "image_2" => "",
            "image_3" => "",
            "image_4" => "",
            "image_5" => ""
        ],
        '_method' => 'PATCH'
    ]
]);
$data = $response->getBody();

// Process the data as per your use case.
```

In the above example code, there is an `id` field in the sole campaign data, so the server will first update the flyer of id **#376918** with the given data. Then the flyer will be added to a new order. If there was no `id` field, then a new flyer would be created and the same will be added to a new order.

The response of the above request will be the details of newly created orders, if all the required fields were submitted. Otherwise, an HTTP `422` error will be returned with the missing field details.

### Success Response

If order creation was successfull an HTTP `200` response with the order details will be returned. A sample response is given below.

```json
{
  "id": 370562,
  "status": "Payment Pending",
  "created_at": "2020-11-04T14:27:31-06:00",
  "cost": 69,
  "discount": 0,
  "tax": 5.69,
  "total": 74.69,
  "use_credits": "0",
  "credits_needed": 69,
  "net_total": 74.69,
  "coupon_name": ""
}
```

The `id` field obtained in this response has to be used to make the order payment.

### Error Response

An HTTP `422` error response will be returned when the flyer validation fails. All the required flyer fields should be present in the request.

A sample error response is given below

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "name": ["The name field is required."],
    "subject": ["The subject field is required."],
    "mail_list_id": ["The mail list id field is required."],
    "prop_price": ["The prop price field is required."]
  }
}
```

Refer the flyer creation doc for the list of required flyer fields.
