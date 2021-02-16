# Retreive a flyer

An authenticated user can retrieve the details of a flyer using the id, if the user has access to it. An owner of the flyer will always have read access to it.

|                |                                               |
| -------------- | --------------------------------------------- |
| Method         | **GET**                                       |
| Endpoint       | https://www.send2sell.com/api/flyer/:id       |
| Parameter      | The id of the flyer that has to be retreived. |
| Authentication | **Required**                                  |

Say, you want to retrieve the details of flyer **#376972**, all you have to do is to make a GET request to the url `https://www.send2sell.com/api/flyer/376972` along with the access token.

An example for the same in PHP is given below

```php
<?php

use GuzzleHttp\Client;

$response = (new Client())->get('https://www.send2sell.com/api/flyer/376972', [
    // Replace `YOUR_ACCESS_TOKEN` with the token you received
    // when performing authentication.
    'headers'  => ['Authorization' => 'Bearer YOUR_ACCESS_TOKEN']
]);
$data = $response->getBody();

// Process the data as per your use case.
```

Send2Sell API will send a JSON response for this endpoint. A sample is shown below. Most of the returned fields are the one's used when creating a flyer. Explanations to others are given below in detail.

```json
{
  "id": 376972,
  "status": "Draft",
  "delivered": 0,
  "opened": 0,
  "clicked": 0,
  "updated_at": "2021-02-09T05:01:19-06:00",
  "edit_allowed": true,
  "prop_address": "1701 Los Osos Valley Rd #5 Los Osos, CA 93402",
  "prop_desc": "<p>Turn key mobile home in every way! This beautiful home has been completely renovated and remodeled in to a super sweet and cozy 1120 sq ft living space in Sunny Oaks Mobile Home Park. Everything is included in this sale. Enter on to a large deck complete with patio furniture and endless views of the Irish Hills and Los Osos Oaks Nature Preserve. Open the french doors to the dining area, living room and kitchen which includes a flat screen TV, custom lighting, matching couch & lounge chairs, double pane windows, all appliances and a custom tile kitchen floor. The bathroom has a tub with shower, Corian counter top & tile flooring. The master bedroom comes completely furnished with an electric fireplace, walk-in closet and sea side decor. Guest room comes with a murphy hide away bed. A laundry room with matching washer/dryer and a shed in the carport for storage and gardening accessories.This mobile home park has entered into a  buy-out program. The mobile home owners in Sunny Oaks are buying the park through a non-profit organization and hope to completely own the park by 2030. The buyer of this home will own an undivided interest in the park when the buy-out is complete. The space rent for a new owner is currently $906.72 monthly.</p>",
  "prop_price": "$189,950",
  "main_image": "/downloads/1612867977-83d2f62bc0b8c971bdf58a9dfb3b7378-full.jpg",
  "cost": 49,
  "headline": null,
  "sub_headline": null,
  "subject": "Open house | 1701 Los Osos Valley Rd #5",
  "rush": "1",
  "scheduled": "2021-02-09T06:29:20-06:00",
  "list_name": "Cuyahoga County, OH",
  "percentage_completed": 1,
  "listing_url": "https://www.send2sell.com/listing/376972/1701-los-osos-valley-rd-5-los-osos-ca-93402",
  "page_views": 0
}
```

## Response field details

#### _status_

|           |            |
| :-------- | :--------- |
| Data type | **String** |

The current status of the flyer. A flyer will always be in either one of these status' - `Draft`, `Cart`, `Scheduled`, `Sending`, `Completed`, and `Cancelled`.

#### _delivered_

|           |             |
| :-------- | :---------- |
| Data type | **Integer** |

The number of subscribers to which the flyer was delivered.

#### _opened_

|           |             |
| :-------- | :---------- |
| Data type | **Integer** |

The number of subscribers that opened the e-flyer.

#### _clicked_

|           |             |
| :-------- | :---------- |
| Data type | **Integer** |

The number of subscribers that clicked on the e-flyer.

#### _edit_allowed_

|           |             |
| :-------- | :---------- |
| Data type | **Boolean** |

This value indicates whether editing the flyer is allowed by the user or not. A flyer of the status `Sending`, `Completed`, or `Cancelled` cannot be edited even if it belongs to the requesting user.

#### _cost_

|           |            |
| :-------- | :--------- |
| Data type | **Number** |

The cost for sending the flyer to the selected mail list.

#### _list_name_

|           |            |
| :-------- | :--------- |
| Data type | **String** |

The name of the selected mail list for delivery.

#### _percentage_completed_

|           |            |
| :-------- | :--------- |
| Data type | **Number** |

A flyer has more than 25 fields that has to be filled before it can be ordered. So, Send2Sell allows partial saving of flyers. The `percentage_completed` field shows what percentage of the total required fields are filled at any time. The value ranges from 0 to 1. Multiply the value with 100 to get the percentage value.

#### _listing_url_

|           |            |
| :-------- | :--------- |
| Data type | **String** |

The landing page url of the flyer. This is where a user clicking on the e-flyer will be taken to.

#### _page_views_

|           |            |
| :-------- | :--------- |
| Data type | **Number** |

The total page views the landing page has received throughout its lifetime.
