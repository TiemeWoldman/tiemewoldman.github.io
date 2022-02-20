> **Portfolio Tieme Woldman - Freelance Technical writer**
> This is an **API documentation** example I wrote. *The API described does not actually exist.*
>
> Or check my [user manual example](https://bit.ly/3ebY2ZX) (Dutch).

<!-- !!! info "Portfolio Tieme Woldman - Freelance Technical writer"
    This is an **API documentation** example I wrote. *The API described does not actually exist.*

    Or check my [user manual example](https://bit.ly/3ebY2ZX) (Dutch). -->

<br>
#  API Reference IBAN Validation
You can use this API to validate an **International Bank Account Number (IBAN)**.

We based the API on [REST](https://restfulapi.net/). It accepts query parameters and returns [JSON](https://www.json.org/json-en.html) responses. Furthermore, it uses standard HTTP response codes.

## Base URL
```HTTP
https://api.tw-applicatiebouw.nl/iban
```

## Endpoint
You use the GET method to validate an IBAN.  

| Method | Endpoint |
|---|---|
| <span id="rcorners" style="background-color:royalblue; color:white">&nbsp;&nbsp;&nbsp;**GET**&nbsp;&nbsp;&nbsp;</span> | `/v2/validate` |

## Request parameters
### 1. Header parameter
You need to specify that the response will be in JSON format.  

| Key | Value | Required/optional |
|---|---|---|
| <span style="color:teal">Content-Type</span> | `application/json` | Required |

### 2. Query string parameters
You need to provide an IBAN that you want to validate. A **Bank Identification Code (BIC)** is optional.

| Parameter | Description | Required/optional | Type |
|---|---|---|---|
| <span style="color:teal">iban</span> | The IBAN to be validated. | Required | String. Maximum of 34 characters. Only digits (0-9) and capital letters (A-Z) are allowed. |
| <span style="color:teal">bic</span> | The BIC of the IBAN to be validated. | Optional | String. Either 8 or 11 characters long. Only digits (0-9) and capital letters (A-Z) are allowed. |

## Request example
This example validates the IBAN `NL99BANK0282915404`. Because this is a fictional IBAN it should fail validation.
```curl
curl -H "Content-Type: application/json" -X POST https://api.tw-applicatiebouw.nl/iban/v2/validate?iban=NL99BANK0282915404
```

## Response example
This is the response to the request above:

```JSON
{
    "iban": "NL99BANK0282915404",
    "bic": "BANKNL3V",
    "valid": False,
}
```
As expected, the validation failed.

You can base your processing on whether an IBAN is valid or not. The response also returns the BIC of your requested IBAN. So, you can also use the API to query for a BIC.

### Response definitions
| Response item | Description | Type |
|---|---|---|
| **iban** | The IBAN you provided for validation. | String |
| **bic** | The BIC of the provided IBAN if valid. | String |
| **valid** | The outcome (True/False) of the validation | Boolean |

## Rate limit
You can make a maximum of 10 requests per minute.

## Status codes
The IBAN Check API uses standard HTTP response codes to signal the success or failure of an API request. Codes in the `2xx` range signal success. Codes in the `4xx` range signal an error that failed given the parameters provided. Codes in the `5xx` range signal an error with our server.

| Status code | Meaning |
|---|---|
| **200 - OK** | The request is OK. |
| **400 - Bad request** | Something is wrong with the parameters. Maybe a required parameter is missing. |
| **429 - Too many requests** | Our API can only handle a maximum of 10 requests per minute. |
| **500 - Server side error** | This one is on us. |

