# Create a new flyer

This document will cover all aspects of a flyer creation on Send2Sell using API. A flyer is tied to a user account, so authentication is required.

|                |                                     |
| -------------- | ----------------------------------- |
| Method         | **POST**                            |
| Endpoint       | https://www.send2sell.com/api/flyer |
| Authentication | **Required**                        |

## Request parameters

These are the parameters supported by the endpoint. Most of them are required fields, some are optional, and some are required only if certain conditions are met. Even though the fields are marked as required, they are required only if the flyer has to be added to cart or needs to be ordered. If a flyer is created without any of these fields, it will be saved as a `Draft` on the platform, which can be edited later.

| Parameter                               | Data type                           | Description                                                                           |
| :-------------------------------------- | :---------------------------------- | :------------------------------------------------------------------------------------ |
| [custom](#custom)                       | Integer                             | **Required.** Accepts only 0 or 1 as values.                                          |
| [custom_flyer](#custom_flyer)           | String                              | **Required if custom is set to 1.** The relative url of an uploaded image.            |
| [template](#template)                   | String                              | **Required if custom is set to 0.** The HTML template name to be used as flyer.       |
| [prop_address](#prop_address)           | String                              | **Required.** Listing address.                                                        |
| [prop_desc](#prop_desc)                 | String                              | **Required.** Listing description.                                                    |
| [prop_details](#prop_details)           | String                              | **Required.** Listing features summary. For example, `2 Beds \| 3 Baths \| 2500 Sqft` |
| [prop_price](#prop_price)               | String                              | **Required.** Listing price.                                                          |
| [main_image](#main_image)               | String                              | **Required.** The relative url of one image of the property.                          |
| [name](#name)                           | String                              | **Required.** Contact name of the listing agent.                                      |
| [email](#email)                         | String                              | **Required.** Contact email address of the listing agent.                             |
| [phone](#phone)                         | String                              | **Required.** Contact phone number of the listing agent.                              |
| [company](#company)                     | String                              | **Required.** Company name of the listing agent.                                      |
| [address_1](#address_1)                 | String                              | **Required.** Street address of the listing agent's company.                          |
| [campaigns](#campaigns)                 | [Array](#campaign-object)           | **Required.** Details of all the email campaigns of this flyer.                       |
| [headline](#headline)                   | String                              | **Optional.** A short header text for the flyers that support it.                     |
| [sub_headline](#sub_headline)           | String                              | **Optional.** A short sub-headline text for the flyers that support it.               |
| [accent_color](#accent_color)           | String                              | **Optional.** RGB hex color code (including the # sign) for flyer accent color.       |
| [theme_color](#theme_color)             | String                              | **Optional.** RGB hex color code (including the # sign) for flyer theme color.        |
| [theme_font_color](#theme_font_color)   | String                              | **Optional.** RGB hex color code (including the # sign) for flyer font color.         |
| [additional_images](#additional_images) | [Object](#additional-images-object) | **Optional.** Additional images to be used in the flyer.                              |
| [website](#website)                     | String                              | **Optional.** The website link that contains more details of the property listing.    |
| [address_2](#address_2)                 | String                              | **Optional.** Additional address line for the agent contact area.                     |
| [headshot](#headshot)                   | String                              | **Optional.** Relative url of the listing agent profile image.                        |
| [license](#license)                     | String                              | **Optional.** License reg. number of the listing agent.                               |
| [logo](#logo)                           | String                              | **Optional.** Relative url of the agent's company logo image.                         |

## Response

A successfull flyer creation will return an array of campaign data.

#### Success response

|                 |       |
| :-------------- | :---- |
| **Status code** | 200   |
| **Response**    | Array |

##### Response data

Response data is an array of campaign details. A single campaign detail will contain the following fields.

| Field     | Data type | Description                                                                  |
| :-------- | :-------- | :--------------------------------------------------------------------------- |
| id        | Integer   | Newly created flyer campaign id. Save this for future references, if needed. |
| list_name | String    | The name of the mail list to which this flyer campaign is scheduled.         |

A sample response data is shown below.

```json
[
  {
    "id": 367356,
    "list_name": "Dallas & Forth Worth, TX"
  },
  {
    "id": 367357,
    "list_name": "Dallas & Forth Worth, TX"
  },
  {
    "id": 367359,
    "list_name": "Austin, TX"
  }
]
```

#### Error response

|                      |                |
| :------------------- | :------------- |
| **Status code**      | 401            |
| **Response message** | Unathenticated |

A `401 Unauthenticated` error response will be send by the server if the user is not authenticated.

For creating a flyer, the requesting user has to be authenticated. Please refer the [authentication API]() docs for more details.

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

#### _template_

|      Required      | Data type |       Supported values       |
| :----------------: | :-------: | :--------------------------: |
| If `custom` is `0` |  String   | `template_1` or `template_2` |

Even though we support custom artwork as a flyer, using one of our HTML template is the recommended approach for better inbox delivery. Right now, we have two email templates `template_1` and `template_2`. Use one of these as the value for `template` parameter.

#### _headline_

| Required | Data type |
| :------: | :-------: |
|    No    |  String   |

Text in this field will be rendered on the headline section of the email templates that support it. Not all templates have a headline section. And this does not have to be filled, if you are using a custom artwork.

A general headline example would look like this - `Open house this Sunday | Free lunch and Wine.`

#### _sub_headline_

| Required | Data type |
| :------: | :-------: |
|    No    |  String   |

This is similar to the headline field except that this will be slightly smaller in font-size.

A general sub-headline example would look like this - `5000 BTSA if closed before Christmas | 4% Commission`

#### _accent_color_

| Required | Data type |
| :------: | :-------: |
|    No    |  String   |

Accent color is a template specific color field that accepts HEX color code as a value. The value should be a valid hex color code including the # sign.

Accent color is usually used to highlight the property address and property features.

#### _theme_color_

| Required | Data type |
| :------: | :-------: |
|    No    |  String   |

Theme color is a template specific color field that accepts HEX color code as a value. The value should be a valid hex color code including the # sign.

Theme color dictates the overall feel of our email templates. Agents usually use it to match it with their company logo.

#### _theme_font_color_

| Required | Data type |
| :------: | :-------: |
|    No    |  String   |

Theme font color is a template specific color field that accepts HEX color code as a value. The value should be a valid hex color code including the # sign.

This is the color of the text that goes along with theme color. Font color should have high visibility on the theme color so that they are visible and easy to the eyes. For a light theme color, choose a dark font color and for a dark theme color, choose a light font color.

#### _prop_address_

| Required | Data type |
| :------: | :-------: |
|   Yes    |  String   |

The address of the property that has to be marketed.

Details like property address, price, description, and an image are required fields on the platform. This allows Send2Sell to create a landing page for each flyer. Users clicking on the e-flyers are taken to this landing page only.

#### _prop_desc_

| Required | Data type |
| :------: | :-------: |
|   Yes    |  String   |

Description of the property.

#### _prop_details_

| Required | Data type |
| :------: | :-------: |
|   Yes    |  String   |

A short feature list of the property. For example, `2 Beds | 3 Baths | 2500 Sqft | 1.25 acre lot`.

> A longer text value might look bad on some of the email templates. So keeping the value of this field under 60 characters would be ideal.

#### _prop_price_

| Required | Data type |
| :------: | :-------: |
|   Yes    |  String   |

The selling/renting price of the property.

#### _main_image_

| Required | Data type |                Supported values                 |
| :------: | :-------: | :---------------------------------------------: |
|   Yes    |  String   | Relative URL to the uploaded image on Send2Sell |

All flyers should have at least one image of the property.

The value of this field should be a relative URL to an uploaded image file. The artwork should be uploaded to Send2Sell using [file uploader]() API.

#### _additional_images_

| Required |              Data type              |
| :------: | :---------------------------------: |
|    No    | [Object](#additional-images-object) |

Send2Sell supports adding six additional images to each flyer. All these six images will be shown in the dedicated flyer landing page, but is not necessary that each image will have a place in the flyer. Some flyers support only four images and some support six. The template that support six image will show all the uploaded six image, if any exists and the template that support four images will show the first four uploaded images.

If no additional images are uploaded, our platform will either leave the space on the template blank or it will completely avoid the section. This will depend on the template you select.

##### Additional images object

The object should contain only the fields given below and the flyer will load only the images on these fields.

| Fields  | Data type | Description                                                                       |
| :------ | :-------- | :-------------------------------------------------------------------------------- |
| image_0 | String    | **Optional.** Relative url of a property image that has to go in the first slot.  |
| image_1 | String    | **Optional.** Relative url of a property image that has to go in the second slot. |
| image_2 | String    | **Optional.** Relative url of a property image that has to go in the third slot.  |
| image_3 | String    | **Optional.** Relative url of a property image that has to go in the fourth slot. |
| image_4 | String    | **Optional.** Relative url of a property image that has to go in the fifth slot.  |
| image_5 | String    | **Optional.** Relative url of a property image that has to go in the sixth slot.  |

You can send fields randomly as given below

```json
{
  //
  // ...other flyer fields
  //
  "additional_images": {
    "image_0": "/uploads/image_0.png",
    "image_2": "/uploads/image_2.png",
    "image_4": "/uploads/image_4.png"
  }
}
```

The above data will fill the slot number one, three, five, and leave all other slots as empty.

#### _name_

| Required | Data type |
| :------: | :-------: |
|   Yes    |  String   |

The name of the property listing agent.

All flyer templates of Send2Sell comes with a contact section. This is where property listing agent details like name, email, and contact number is specified. This is also used in the flyer landing page on our platform.

#### _email_

| Required | Data type |
| :------: | :-------: |
|   Yes    |  String   |

The email address of the property listing agent.

#### _phone_

| Required | Data type |
| :------: | :-------: |
|   Yes    |  String   |

The contact number of the property listing agent.

#### _headshot_

| Required | Data type |
| :------: | :-------: |
|    No    |  String   |

The relative URL to an uploaded image file of agent headshot. Flyer landing page and some of the email templates have provision for showing the agent headshot, this image goes in there, if supported.

#### _license_

| Required | Data type |
| :------: | :-------: |
|    No    |  String   |

This field allows brokers/agents to enter their license number. Some states and/or realtor associations require this by law.

#### _company_

| Required | Data type |
| :------: | :-------: |
|   Yes    |  String   |

The company name of the property listing agent.

Just like agent contact details, agent broker details are also part of each and every flyer send from our platform. This includes broker name, address, and logo.

#### _address_1_

| Required | Data type |
| :------: | :-------: |
|   Yes    |  String   |

The street address of the broker/agent where mails can be delivered.

#### _address_2_

| Required | Data type |
| :------: | :-------: |
|    No    |  String   |

An additional address line for specifying the city, state, and zip code of the listing agent/broker. Something like `Dallas, TX 75001`

#### _campaigns_

| Required | Data type |
| :------: | :-------: |
|   Yes    |   Array   |

Campiagns field is an essential field for adding the flyer to a cart or for ordering it and hence each flyer should contain at least one [campaign object](#campaign-object). Campaigns tell Send2Sell the email delivery details like subject line, the mail list to which the flyer has to be delivered, delivery time etc. The field accepts an array of campaign objects, so that same flyer can be send to multiple lists or at multiple times.

A campaigns array will look like

```json
{
  //
  // ... other flyer fields
  //
  "campaigns": [
    {
      "subject": "6405 Forest Ln, Dallas TX | Open house this Sunday",
      "mail_list_id": 8,
      "rush": 0,
      "scheduled": "02/13/2021 09:00 AM CST"
    },
    {
      "subject": "6405 Forest Ln, Dallas TX | Open house this Sunday",
      "mail_list_id": 8,
      "rush": 1,
      "scheduled": "02/12/2021 09:00 AM CST"
    },
    {
      "subject": "Condo in Austin | 5000 BTSA",
      "mail_list_id": 2,
      "rush": 1,
      "scheduled": "02/12/2021 09:00 AM CST"
    }
  ]
}
```

The above data contains the details for three campaigns to two seperate lists. Two of them are to same list but scheduled for two different delivery times. All these campaigns will be delivered on the choosen delivery time on successfull payment of the order.

##### Campaign object

The fields supported in a single campaign object is given below.

| Fields                        | Data type | Description                                                                                |
| :---------------------------- | :-------- | :----------------------------------------------------------------------------------------- |
| [subject](#subject)           | String    | **Required.** The email subject field of the campaign.                                     |
| [mail_list_id](#mail_list_id) | Integer   | **Required.** The id of the mail list to which the campaign has to be send.                |
| [rush](#rush)                 | Integer   | **Required.** Send2Sell has a standard order and rush order. Supported values are 0 and 1. |
| [scheduled](#scheduled)       | String    | **Required.** The time in CST at which the campaign has to be delivered.                   |

#### _subject_

| Required | Data type |
| :------: | :-------: |
|   Yes    |  String   |

This is the email subject for the flyer campaign. An example would be `6405 Forest Ln, Dallas TX | Open house this Sunday`. Keep it short and simple and avoid using `!` and `$` characters. These characters will be automatically stripped by our email servers before delivery.

#### _mail_list_id_

| Required | Data type |
| :------: | :-------: |
|   Yes    |  Integer  |

This field determines the mail list to which a campaign has to be send. The value should be a valid id of one of our mail list. You can retreive all the mail list details using our [Mail List API]().

#### _rush_

| Required | Data type | Supported values |
| :------: | :-------: | :--------------: |
|   Yes    |  Integer  |      0 or 1      |

Send2Sell supports two kinds of campaigns - Rush campaign and Standard campaign. Standard campaigns have a processing delay of 48 hours ie, the campaign will be send only at a time of your choosing after 48 hours. Rush orders on the other hand has a processing delay of only 90 minutes and the campaign is delivered exactly at the time of your choosing.

Sending a consistent number of emails throughout a day is really important for better deliverability. Standard orders helps us with that by giving us the flexibility to change the delivery time of a campaign to a better slot.

Set the value of this field to `1` for opting it as a rush campaign and `0` for standard campaign.

#### _scheduled_

| Required | Data type |
| :------: | :-------: |
|   Yes    |  String   |

This is the time in CST/CDT at which the campaign has to be delivered. Send2Sell operates on CST/CDT timezone and expects all the time input to be in the same zone.

The supported value of this field depends on the value of [`rush`](#rush) field. If the campaign is a rush campaign, then this delivery time can be any time after 90 minutes from now. And, if it is a standard campaign, then it should be a time after 48 hours from now. If the delivery time is set to anytime earlier than the supported value, they will be adjusted automatically to the said range.

For example, say the current time is `02/11/2021 09:00 AM CST` and you set the value of `rush` to `0` and choose to schedule the campaign for delivery at 24 hours from now - `02/12/2021 09:00 AM CST`. Our server will see this as an invalid input, and automatically adjusts the delivery time to `02/13/2021 09:00 AM CST` ie 48 hours from the current time.
