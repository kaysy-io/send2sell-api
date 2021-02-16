# How to upload files using Send2Sell API?

Our email delivery service require all the flyer images to be stored within our servers for better inbox delivery. So it is really important that the API users upload flyer images to our servers for use on flyers. This guide will help you to upload files using the API.

|                |                                      |
| -------------- | ------------------------------------ |
| Method         | **POST**                             |
| Endpoint       | https://www.send2sell.com/api/upload |
| Authentication | **Required**                         |

This file uploader endpoint supports the content type `multipart/form-data` enabling authenticated users to upload files. Even though the endpoint support file uploads, there are some strict security restrictions in place. These are listed below -

### 1. All the upload request should have an `upload_name` field

Send2Sell requires some of the images uploaded to be resized in a specific manner. For example, a flyer `main_image` file has to be resized to 1248px width and `additional_images` to 600px width. Similarly there are different file sizes for different images that goes into a flyer. Since, these images won't be used anywhere else, we resize and store only the version that is needed in the flyer.

The `upload_name` field tells the API which image is being uploaded and how to resize them. The value of this field should be same as the form field name of the uploaded file. The image field names that goes into a flyer are given below. These should be used as such for resizing them.

#### _main_image_

The main image that goes into the flyer. This image will be resized into 1248px x 768px.

#### _custom_flyer_

The custom flyer artwork image that should be used as the flyer for delivery. This image will be resized into 600px width and the height will be auto adjusted keeping the aspect ratio same as the original. Only this upload name supports PDF uploads. If a PDF is uploaded, it will be convereted into a JPEG image by the server, which in turn will be used as the flyer for delivery.

#### _image_0_

The additional image that goes into a flyer. This image will be resized into 600px x 400px.

#### _headshot_

The headshot image of the agent that goes into the flyer. This image will be resized into 568px x 600px.

#### _logo_

The logo image of the agents company that goes into the flyer. This image will be resized into 600px x 240px.

These are the flyer fields that needs resizing and hence when uploading an image goes into either of these fields, it should use the `upload_name` as the flyer form field name.

### 2. Maximum allowed flie size is 8192 KiloBytes

Send2Sell server limits upload size to 8MB. Everything above it will throw an HTTP `422` error code.

### 3. Only limited file types are allowed

Send2Sell server supports only images of JPEG/JPG/PNG format. If it is a `custom_flyer` upload, then the PDF is also supported. Any other upload will result in HTTP `422` error response.

## A file upload example in PHP

The code given below shows you how to upload a file for the `main_image` field of a flyer.

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

As you can see the request contains a multipart data, where one part is the `upload_name` field with its value. In this case `main_image`. The other is raw file data with the form field name `main_image` - the same one used as the value of the field `upload_name`. Its content is the raw data of file to be uploaded. The `filename` field tells the Send2Sell server, the name and extension of the uploaded file.

## Response to the above example

If the upload and resize was successfull, the API will return an array containing the relative url of the uploaded image. The URL is relative to the Send2Sell domain. You can save it, if required, or carry the result forward to other process' that actually require the value of the uploaded file location.

If the uploaded file was within the size limits, the above request would have returned a response similar to

```json
["main_image" => "/uploads/156790393-open-house-main-image.png"]
```

`main_image` is the value of the `upload_name` used in the request.

#### An error response

HTTP errors with status code `422` will be returned if the uploaded file is not supported by the server. Say, the image you tried to upload was above 8MB in size. In that case the response would be

```json
[
    "errors" => [
        "main_Image" => ["Main image cannot be more than 8MB in size."]
    ]
]
```
