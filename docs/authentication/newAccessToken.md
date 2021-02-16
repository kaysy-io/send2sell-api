# Retreive new access token

Many of the core features of the Send2Sell API needs authentication. This is done using access tokens. An access token is issued when a client requests one using their API key and secret code. Lifetime of an access token is limited to 12 hours.

> **Note:** Your API keys should be kept private or anyone with access to it can compromise your account. So you should make authentication requests only from the server side. Also avoid hard coding the API keys on the code and commiting it to remote repositories.

|                |                                         |
| -------------- | --------------------------------------- |
| Method         | **GET or POST**                         |
| Endpoint       | https://www.send2sell.com/api/authorize |
| Authentication | **Not required**                        |

## Request parameters

For generating a new access token, the request should contain the client API key and secret code. For the time being, the keys will be issued only on request after reveiwing the application. In future, you will be able to access this from your account dashboard. [Contact us for API keys.](https://www.send2sell.com/contact)

| Parameter                       | Data type | Required |
| :------------------------------ | :-------- | :------: |
| [client_id](#client_id)         | String    | **Yes**  |
| [client_secret](#client_secret) | String    | **Yes**  |

## Response

A new access token object will be returned on a valid request.

#### Success response

|                 |                                             |
| :-------------- | :------------------------------------------ |
| **Status code** | 200                                         |
| **Response**    | [Access Token Object](#access-token-object) |

##### Access token object

| Field     | Data type | Description                                                                                 |
| :-------- | :-------- | :------------------------------------------------------------------------------------------ |
| ip        | String    | IP address of the server that requested this new token.                                     |
| api_token | String    | The token to be used along with all requests.                                               |
| expiry    | String    | The time in UTC at which this token expires.                                                |
| client_id | Integer   | The id of the requesting API client.                                                        |
| user_id   | Integer   | The id of the authenticated user. Usually the user to which the API credentials belongs to. |
| id        | Integer   | The id of this access token.                                                                |

A sample response object will look like the one given below.

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

#### Error response

A 403 error response will be returned when API credentials are missing or is invalid.

|                      |                                                                                                                      |
| :------------------- | :------------------------------------------------------------------------------------------------------------------- |
| **Status code**      | 403                                                                                                                  |
| **Response message** | - Request missing client id.<br>- Invalid client id.<br>- Invalid API client credentials.<br>- Error creating token. |

## Request parameter details

A Send2Sell API key is a combination of two keys - `client_id` and `client_secret`. Together they make an API key pair. Both the `client_id` and `client_secret` is required to authenticate a user into the application. [Contact us for obtaining these keys.](https://www.send2sell.com/contact)

#### _client_id_

| Required | Data type |
| :------: | :-------: |
|   Yes    |  String   |

An alphanumeric string with 32 characters.

#### _client_secret_

| Required | Data type |
| :------: | :-------: |
|   Yes    |  String   |

An alphanumeric string with 64 characters.
