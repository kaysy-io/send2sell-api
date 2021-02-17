# How to delete a flyer?

A flyer can be deleted by an authenticated user if the flyer belongs to the user and its status is either `Draft` or `Cart`.

|                |                                     |
| -------------- | ----------------------------------- |
| Method         | **DELETE**                          |
| Endpoint       | https://www.send2sell.com/api/flyer |
| Authentication | **Required**                        |

## Sample delete request

All flyer delete request should contain an `id` field specifying the **id** of the flyer that has to be deleted.

> If your client does not support sending an HTTP `DELETE` request, a `_method` field with the value **DELETE** should be sent along with the other data in a `POST` request.

A sample code in PHP will look like the one given below.

```php
<?php

use GuzzleHttp\Client;

$response = (new Client())->post('https://www.send2sell.com/api/flyer', [
    // Replace `YOUR_ACCESS_TOKEN` with the token you received
    // when performing authentication.
    'headers'  => ['Authorization' => 'Bearer YOUR_ACCESS_TOKEN'],
    'form_params' => [
        'id' => 3567890,
        '_method' => 'DELETE'
    ]
]);
```

The above sample code is requesting the Send2Sell API server to delete a flyer of the id `3567890`. On successfull deletion, the response will be an HTTP `200` code with no response data.

## Error responses

|                      |                                                                  |
| :------------------- | :--------------------------------------------------------------- |
| HTTP status code     | **403**                                                          |
| Sample response data | `{ message : "You are not authorized to delete this product." }` |

If the user is not authenticated to delete a flyer, an HTTP `403` error response stating `You are not authorized to delete this product.` will be returned.

If the flyer can't be deleted if it's status doesn't allow deletion, then an HTTP `403` error response stating `Only drafts or flyers in cart can be deleted.` will be returned.
