# How to use access tokens with requests?

Access token is how Send2Sell API determines the authenticity of a request. This token should be submitted along with all the requests that requires user authentication. This doc will help you retrieve a new access token, and how authentication has to be carried out.

### Retreive an access token from the API.

To retreive a new access token, please refer the **[Retreive new access token]()** guide.

A sample access token response object will look like the one given below.

```json
{
  "ip": "3.168.222.5",
  "api_token": "ad6ENeaQr9pXhGhWQsBZeqxYMuv5aIz9OwUgcX4pF2OPcEr0NeyPkOda2ypQ",
  "expiry": "2021-02-11T23:47:07.907279Z",
  "client_id": 2,
  "user_id": 53,
  "id": 1520189
}
```

`ad6ENeaQr9pXhGhWQsBZeqxYMuv5aIz9OwUgcX4pF2OPcEr0NeyPkOda2ypQ` is the access token in the above sample. This value has to be used in all requests that requires authentication. The `expiry` value tells you the time at which this token becomes invalid. Either you can store the expiry value and issue a new token in advance to avoid any API errors after or it is absolutely fine to create a new one when a `401` exception is thrown by Send2Sell API.

> Send2Sell API sends a 401 HTTP response code when a request requires authentication and the requesting user is not authenticated.

### How to use the access token with a request?

Send2Sell API checks the access token in three places -

1. Query field `access_token`.
2. Request form body field `access_token`.
3. Authorization bearer token.

#### 1. Access token in the query field

This is one of the easiest way to send access token to Send2Sell API. Send the `access_token` as a query field to any authentication requiring endpoint and Send2Sell API will handle it gracefully.

For example, to retreive the flyer **#376972**, use the endpoint `https://www.send2sell.com/api/flyer/376972?access_token=YOUR_ACCESS_TOKEN`

#### 2. Access token in the request form parameters

In this case, instead of submitting the access token as a query field, we will submit it along with other form data.

An example code in PHP to upload a file is given below

```php
<?php

use GuzzleHttp\Client;

$response = (new Client())->post('https://www.send2sell.com/api/upload', [
    'multipart' => [
        [
            'name'     => 'access_token',
            'contents' => 'YOUR_ACCESS_TOKEN',
        ],
        [
            'name'     => 'upload_name',
            'contents' => 'main_image',
        ],
        [
            'name'     => 'main_image',
            'contents' => fopen('/path/to/file', 'r'),
            'filename' => 'open-house-main-image.png'
        ],
    ]
]);
$data = $response->getBody();

// Process the data according to your use case.
```

As you can see, the field `access_token` is send as a part of the request form field instead of submitting it as a URL query string. This will successfully pass the authentication checks, if the access token is valid.

#### 3. Access token in the `Authorization` header

Send2Sell server supports access token in the `Authorization` header. The above example of file upload can be refactored as given below.

```php
<?php

use GuzzleHttp\Client;

$response = (new Client())->post('https://www.send2sell.com/api/upload', [
    // Replace `YOUR_ACCESS_TOKEN` with the token you received
    // when performing authentication.
    'headers'  => ['Authorization' => 'Bearer YOUR_ACCESS_TOKEN'],
    'multipart' => [
        [
            'name'     => 'upload_name',
            'contents' => 'main_image',
        ],
        [
            'name'     => 'main_image',
            'contents' => fopen('/path/to/file', 'r'),
            'filename' => 'open-house-main-image.png'
        ],
    ]
]);
$data = $response->getBody();

// Process the data according to your use case.
```

Note the additional headers field in the request. This looks cleaner and is the recommended approach mainly because of the fact the most HTTP client library allow setting global header values. Once an access token is retreived from the Send2Sell API, the user can set the Authorization header as a global header and never worry again about setting the access token on every request.
