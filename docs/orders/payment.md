# How to pay for an order?

A user can pay for an order using the Send2Sell API only if they have enough prepaid credit balance. Otherwise, the order has to be paid from the Send2Sell web application itself.

|                |                                                 |
| -------------- | ----------------------------------------------- |
| Method         | **PATCH**                                       |
| Endpoint       | https://www.send2sell.com/api/order/:id/billing |
| Parameter      | The id of the order that has to be paid.        |
| Authentication | **Required**                                    |

This endpoint needs the complete order billing details submitted to it. Send2Sell server will save the complete billing details first and then try to consume prepaid credits. If credit consumption was successfull, the order will be marked as `Paid`. Otherwise an HTTP `402 Payment Required` error will be thrown.

### Request for credit payment

A sample request for order payment in PHP is shown below

```php
<?php

use GuzzleHttp\Client;

$response = (new Client())->post('https://www.send2sell.com/api/order/370562/billing', [
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

> **NOTE:** The `use_credits` field should be `1` inorder to pay an order via prepaid credits, which is the only available payment options for API.

### Success response

If the order payment was successfull using prepaid credits, an HTTP `200` error with the order details will be returned. A sample response is given below.

```json
{
  "id": 370562,
  "status": "Paid",
  "created_at": "2020-11-04T14:27:31-06:00",
  "cost": 69,
  "discount": 0,
  "tax": 0,
  "total": 0,
  "use_credits": "1",
  "consumed_credits": 69,
  "payment_method": "Prepaid credits",
  "credits_needed": 0,
  "net_total": 0,
  "coupon_name": ""
}
```

> **NOTE:** A payment successfull order will have the status `Paid`.

### Payment Required Error Response

If the authenticated user doesn't have enough credits to pay for the order, an HTTP `402 Payment Required` error will be returned with the order details as the body. A sample response will look like the one given below.

```json
{
  "id": 370562,
  "status": "Payment Pending",
  "created_at": "2020-11-04T14:27:31-06:00",
  "cost": 69,
  "discount": 0,
  "tax": 5.69,
  "total": 74.69,
  "use_credits": "1",
  "credits_needed": 69,
  "net_total": 74.69,
  "coupon_name": ""
}
```

In the above response, it is clear that even though the `use_credits` field is `1`, Send2Sell was unable to capture credits from the user account. So a total of $74.69 has to be collected from the user before the order can be marked as `Paid`.

### Other error responses

In addition to the `Payment Required` error explained above, there are few other errors that would be thrown when a user attempts to make credit payments.

##### Unauthorized access error

An HTTP `403` error will be thrown when the user has no permission to access the requested order.

```json
{
  "message": "You are not authorized to access this order."
}
```

##### Order does not allow payment

An HTTP `403` error will be thrown when the requested order was already paid or when no further payment updates are allowed.

```json
{
  "message": "This order does not allow payment. Please contact us."
}
```

##### No products linked to the order

An HTTP `403` error will be thrown when the requested order has no products linked to it.

```json
{
  "message": "No cart or products linked to this order."
}
```
