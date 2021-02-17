# How to update a flyer?

To update a flyer, the user has to first retreive the flyer for edit purpose. This is different from normal retreival as mentioned in the `retreive.md` guide. That one is for display purpose only which include just the flyer summary and stats/analytics.

## Retreive flyer for edit

|                |                                              |
| -------------- | -------------------------------------------- |
| Method         | **GET**                                      |
| Endpoint       | https://www.send2sell.com/api/flyer/:id/edit |
| Parameter      | The id of the flyer that has to be updated.  |
| Authentication | **Required**                                 |

Say, you want to retrieve the details of flyer **#376918**, all you have to do is to make a GET request to the url `https://www.send2sell.com/api/flyer/376918/edit` along with the access token.

An example for the same in PHP is given below

```php
<?php

use GuzzleHttp\Client;

$response = (new Client())->get('https://www.send2sell.com/api/flyer/376918/edit', [
    // Replace `YOUR_ACCESS_TOKEN` with the token you received
    // when performing authentication.
    'headers'  => ['Authorization' => 'Bearer YOUR_ACCESS_TOKEN']
]);
$data = $response->getBody();

// Process the data as per your use case.
```

Send2Sell API will send an HTTP `200` JSON response for this endpoint. A sample response is shown below. The returned fields are the one's used when creating a flyer. Please refer the flyer creation doc for the explanation of each fields.

```json
{
  "campaigns": [
    {
      "mail_list_id": null,
      "subject": null,
      "rush": null,
      "scheduled": "2021-02-17T05:46:39-06:00",
      "id": 376918
    }
  ],
  "name": "Rebecca White",
  "license": null,
  "company": "Intero Real Estate Services",
  "address_1": "2488 Junipero Serra Blvd",
  "address_2": "Daly City, CA 94015",
  "email": "RebeccaW@Intero.com",
  "phone": "4154121977",
  "headshot": null,
  "logo": null,
  "prop_address": "59 Westfield Avenue Daly City, CA 94015",
  "prop_desc": "<p>Gorgeous remodeled Westlake Highlands home with 4 bedrooms and 3 bathrooms.Immaculate move-in condition! Open kitchen with beautiful cherry kitchen cabinets, stainless appliances and granite counters. Hardwood floors, crown molding and recessed lights throughout. Upper/main level has 2 bedrooms and 1 bathroom. Downstairs has 2 bedrooms and 2 bathrooms plus a family/living room. Laundry appliances in garage.Fabulous 3-level, terraced backyard full of lovely flowers, apple trees and lemon trees that produce fruit year 'round. Newly added garden shed for extra storage. Close to Westlake for all of your shopping and dining pleasure, Lake Merced Golf Course, Westmoor High School, Mussel Rock &amp; Longview Parks, Civic Center, and Great Highway. Virtual Open Houses: Saturday, November 7, 1-4 pm and Sunday, November 1 and 8, 1-4 pm&nbsp;<br>Calendly link<br>Zoom link: &nbsp;<a href=\"https://zoom.us/j/97736601984\">https://zoom.us/j/97736601984</a><br>&nbsp;</p>",
  "prop_price": "$1,200,000",
  "prop_details": "4 bedrooms, 3 full bathrooms, 1610 square feet, attached garage, yard,",
  "main_image": "/downloads/1604534765-39628710fb4020bf76c51c72e4d4ce31-full.jpg",
  "custom": "0",
  "theme_color": "#444444",
  "theme_font_color": "#FFFFFF",
  "accent_color": "#7D0F0F",
  "headline": "Virtual Open Houses/Showings by Appointment Saturday & Sunday 1 pm-4 pm",
  "sub_headline": "4 Bed, 3 Bath Westlake Highlands Home with Bonus Rooms and FABULOUS Yard!",
  "template": "template_1",
  "additional_images": {
    "image_0": "",
    "image_1": "",
    "image_2": "",
    "image_3": "",
    "image_4": "",
    "image_5": ""
  }
}
```

## Update request

|                |                                     |
| -------------- | ----------------------------------- |
| Method         | **PATCH**                           |
| Endpoint       | https://www.send2sell.com/api/flyer |
| Authentication | **Required**                        |

Even though we are updating just one or two fields, Send2Sell requires the whole flyer data to be submitted back with the changes included. Failing to do so will result in an HTTP `422` error.

In the sample example given below, we will be changing just the flyer template field from `template_1` to `template_2`.

> Send2Sell performs updates based on the **id** field in the campaigns data of the flyer. This is why there is no **id** parameter in the endpoint URL.
>
> So all update requests should have a campaigns array with one campaign item and the `id` field of this campaign item should be the same as the one retreived from the server.

```php
<?php

use GuzzleHttp\Client;

$response = (new Client())->get('https://www.send2sell.com/api/flyer/376918/edit', [
    // Replace `YOUR_ACCESS_TOKEN` with the token you received
    // when performing authentication.
    'headers'  => ['Authorization' => 'Bearer YOUR_ACCESS_TOKEN'],
    'form_params' => [
        "campaigns" => [
            [
                "mail_list_id" => null,
                "subject" => null,
                "rush" => null,
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

On success, the response of this update will be an HTTP `200` status code with the flyer summary.

### Error response

An HTTP `403` response will be returned if the authenticated user is not allowed to update the flyer.

```json
{
  "message": "You are not authorized to make changes to this product."
}
```
