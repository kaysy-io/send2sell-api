# Create a new flyer

This document will cover all aspects of a flyer creation on Send2Sell using API. A flyer is tied to a user account, so authentication is required.

|                |                                     |
| -------------- | ----------------------------------- |
| Method         | **POST**                            |
| Endpoint       | https://www.send2sell.com/api/flyer |
| Authentication | **Required**                        |

## Request parameters

These are the parameters supported by the endpoint. Most of them are required fields, some are optional, and some are required only if certain conditions are met. Even though the fields are marked as required, they are required only if the flyer has to be added to cart or needs to be ordered. If a flyer is created without any of these fields, it will be saved as a `Draft` on the platform, which can be edited later.

| Parameter         | Data Type | Description                                                                           |
| ----------------- | --------- | ------------------------------------------------------------------------------------- |
| custom            | Integer   | **Required.** Accepts only 0 or 1 as values.                                          |
| custom_flyer      | String    | **Required if custom is set to 1.** The relative url of an uploaded image.            |
| template          | String    | **Required if custom is set to 0.** The HTML template name to be used as flyer.       |
| prop_address      | String    | **Required.** Listing address.                                                        |
| prop_desc         | String    | **Required.** Listing description.                                                    |
| prop_details      | String    | **Required.** Listing features summary. For example, `2 Beds \| 3 Baths \| 2500 Sqft` |
| prop_price        | String    | **Required.** Listing price.                                                          |
| main_image        | String    | **Required.** The relative url of one image of the property.                          |
| name              | String    | **Required.** Contact name of the listing agent.                                      |
| email             | String    | **Required.** Contact email address of the listing agent.                             |
| phone             | String    | **Required.** Contact phone number of the listing agent.                              |
| company           | String    | **Required.** Company name of the listing agent.                                      |
| address_1         | String    | **Required.** Street address of the listing agent's company.                          |
| campaigns         | Array     | **Required.** Details of all the email campaigns of this flyer.                       |
| headline          | String    | **Optional.** A short header text for the flyers that support it.                     |
| sub_headline      | String    | **Optional.** A short sub-headline text for the flyers that support it.               |
| accent_color      | String    | **Optional.** RGB hex color code (including the # sign) for flyer accent color.       |
| theme_color       | String    | **Optional.** RGB hex color code (including the # sign) for flyer theme color.        |
| theme_font_color  | String    | **Optional.** RGB hex color code (including the # sign) for flyer font color.         |
| additional_images | Object    | **Optional.** Additional images to be used in the flyer.                              |
| website           | String    | **Optional.** The website link that contains more details of the property listing.    |
| address_2         | String    | **Optional.** Additional address line for the agent contact area.                     |
| headshot          | String    | **Optional.** Relative url of the listing agent profile image.                        |
| license           | String    | **Optional.** License reg. number of the listing agent.                               |
| logo              | String    | **Optional.** Relative url of the agent's company logo image.                         |

## Request parameter details

#### _custom_

| Required | Data type | Supported values |
| :------: | :-------: | :--------------: |
|   Yes    |  Integer  |      0 or 1      |

Send2Sell supports sending custom flyer artwork using our platform. The artwork should be a PNG/JPEG/JPG image file under 8MB in size. To send a custom flyer artwork, set the value of `custom` to `1`.

If you don't have a custom artwork, you can use one of our email templates. To use our own email templates, set the value of the `custom` to `0`.

#### _custom_flyer_

|      Required      | Data type |                Supported values                 |
| :----------------: | :-------: | :---------------------------------------------: |
| If `custom` is `1` |  String   | Relative URL to the uploaded image on Send2Sell |

This is the URL to your custom flyer artwork. The artwork should be uploaded to Send2Sell using [file uploader]() API. The URL should be a relative URL to the Send2Sell platform for better inbox delivery of the flyers. So external image links are not supported. [Refer the file uploader API for more details.]()


'rush' => true,
'scheduled' => '',
'subject' => '',

'mail_list_id' => 'bail|required|exists:mail_lists,id',
'rush' => 'required|boolean',
'scheduled' => 'required|date',
'subject' => 'required|string',
