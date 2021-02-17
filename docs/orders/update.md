# Update an order

An authenticated user can update the details of an order using the id, if the user has access to it and can be updated. The order details can be updated only if the status is either `Payment Pending` or `Partially Paid`.

|                |                                            |
| -------------- | ------------------------------------------ |
| Method         | **PATCH**                                  |
| Endpoint       | https://www.send2sell.com/api/order/:id    |
| Parameter      | The id of the order that has to be updated |
| Authentication | **Required**                               |

This endpoint can be used to update order billing details and to authorize prepaid credit usage.

> If your client does not support sending an HTTP `PATCH` request, a `_method` field with the value **PATCH** should be sent along with the other data in a `POST` request.

### Request for updating

A sample request for updating the order details of **#370562** in PHP is shown below

```php
<?php

use GuzzleHttp\Client;

$response = (new Client())->post('https://www.send2sell.com/api/order/370562', [
    // Replace `YOUR_ACCESS_TOKEN` with the token you received
    // when performing authentication.
    'headers'  => ['Authorization' => 'Bearer YOUR_ACCESS_TOKEN'],
    'form_params' => [
        'first_name' => 'BILLING_USER_FIRST_NAME',
        'city' => 'BILLING_USER_CITY',
        'state' => 'TX',
        'zip' => 'BILLING_USER_ZIP_CODE',
        'phone' => 'BILLING_USER_PHONE',
        'use_credits' => '0',
        '_method' => 'PATCH'
    ]
]);
```

The `first_name`, `city`, `state`, `zip`, and `phone` fields are manadatory in every order update requests. Please refer the [request parameter details](#request-parameter-details) for more info.

### Success response

The success response for the above request will look like the one given below

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

In the above request, we have updated the `use_credits` field to 0 ie, we have set it not to use any prepaid credits for paying the order. So, the user has to pay a total of **$74.69** for placing the order. This can be done only from the Send2Sell web application.

### Update the order details to use prepaid credits

Say, you have enough pepaid credit balance and wants to place orders using the Send2Sell API. In that case, all you have to do is to update the order details with a `use_credits` field set to `1`.

A sample update request in PHP is given below.

```php
<?php

use GuzzleHttp\Client;

$response = (new Client())->post('https://www.send2sell.com/api/order/370562', [
   // Replace `YOUR_ACCESS_TOKEN` with the token you received
   // when performing authentication.
   'headers'  => ['Authorization' => 'Bearer YOUR_ACCESS_TOKEN'],
   'form_params' => [
       'first_name' => 'BILLING_USER_FIRST_NAME',
       'city' => 'BILLING_USER_CITY',
       'state' => 'TX',
       'zip' => 'BILLING_USER_ZIP_CODE',
       'phone' => 'BILLING_USER_PHONE',
       'use_credits' => '1',
       '_method' => 'PATCH'
   ]
]);
```

When the user has enough prepaid credits to pay for the order, response of the above request will be

```json
{
  "id": 370562,
  "status": "Payment Pending",
  "created_at": "2020-11-04T14:27:31-06:00",
  "cost": 69,
  "discount": 0,
  "tax": 0,
  "total": 0,
  "use_credits": "1",
  "credits_needed": 69,
  "net_total": 0,
  "coupon_name": ""
}
```

As you can see, the order total was updated when it is authorized to consume prepaid credits of the billing user. When an order payment request is made later, `69` credits will be debited from the user account and the order will be marked as `Paid`.

### Error response

If the requesting user has no permission to update the order, an HTTP `403` error response will be returned

```json
{
  "message": "You are not authorized to update this order."
}
```

### Request parameter details

#### _first_name_

| Data type  | Required |
| :--------- | :------- |
| **String** | **Yes**  |

First name of the billing user.

#### _last_name_

| Data type  | Required |
| :--------- | :------- |
| **String** | **No**   |

Last name of the billing user.

#### _street_

| Data type  | Required |
| :--------- | :------- |
| **String** | **No**   |

Street name of the order billing address.

#### _city_

| Data type  | Required |
| :--------- | :------- |
| **String** | **Yes**  |

City name of the order billing address.

#### _state_

| Data type  | Required |
| :--------- | :------- |
| **String** | **Yes**  |

Two letter state code of the order billing address. [Refer the states API for retreiving two letter codes.](https://github.com/kaysy-io/send2sell-api/blob/main/docs/states/retreive.md)

#### _zip_

| Data type  | Required |
| :--------- | :------- |
| **String** | **Yes**  |

Zip code of the order billing address.

#### _phone_

| Data type  | Required |
| :--------- | :------- |
| **String** | **Yes**  |

Contact number of the billing user.

#### _use_credits_

| Data type  | Required |
| :--------- | :------- |
| **String** | **No**   |

Supported values are `0` or `1`. Set the value to `1` inorder to use the prepaid credits of the authenticated user.
