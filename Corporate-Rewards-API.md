# YouGotaGift.com Corporate Rewards API _v1.0_

![YouGotaGift.com Logo](https://yougotagift.com/static/img/yougotagift.png)

## Overview

This is a documentation of the YouGotaGift.com Corporate Rewards API 1.0

### API Older Version Links
[Version 0.4](https://github.com/YouGotaGift/docs/blob/master/corporate-rewards-API-v0.4.md)

[Version 0.3](https://github.com/YouGotaGift/docs/blob/master/corporate-rewards-API-0.3.md)

[Version 0.2](https://github.com/YouGotaGift/docs/blob/master/corporate-rewards-API-0.2.md)

### Changes
Added support for international brands which requires additional details for redemption. Eg: Code, Redemption URL, PIN etc..

### Summary of changes
* Removed "code" , instead introduced "gift_voucher" which provides all the important gift card details eg: code, redemption url, Pin etc..  
* Removed "pdf_link" since "gift_pdf_link" has been introduced in 0.4 to provide a direct gift pdf download link.
* Removed "excel_link" since this functionality is currently not used by any user. Might introduce later when the need arises.
* Removed "total_amount" since the gift amount already holds the value
* Removed "count" since it always returns 1 and more than one quantity is not allowed
* "gifts_json" key returned by download endpoint has been renamed to "gift_json"

### How It Works

The YouGotaGift.com Corporate Rewards API is an HTTP API, you can call it with simple HTTP GET/POST, and the result will be in JSON.

### API Endpoints
* All the following API endpoints are available under `https://yougotagift.com/corporate/api/`.
* All the API points return JSON responses.
* All the API points are callable using HTTP methods, some will only accept `GET` requests, some will only accept `POST` requests, and some will accept both.
* Authentication is done using HTTP Basic Authentication over TLS secured channel in HTTPS.
* The HTTP Response Code will tell you whether your call was successful or not.
* `HTTP 200` Means everything went okay.
* `HTTP 404` Means the object you were accessing does not exist.
* `HTTP 400` Means that you have an error in your request, the JSON content will have `errors` field which will explain what is the issue.
* `HTTP 500` Oops, it's our fault, we'll solve it ASAP.

#### `download`
- **Endpoint** `https://yougotagift.com/corporate/api/incentives-send/download/`
- **Returns** JSON Object with the result of your request
- **Accepts** `POST` only.
- **Request format** Should be an HTTP POST with a JSON array in the body of JSON objects for gifts requested.
- **Requires Authentication**

##### JSON Payload Parameters

| Parameter    | Description   |
| ------------ | ------------- |
| brand | Brand name.  **Required**.  Up-to-date brand names list can be found at https://yougotagift.com/gift-card-mall/all-brands/ to access other countries lists check the end of this file. |
| country | The brand's country.  Possible values: `AE`, `LB`, `SA`, `QA`, `UK`, `US`. _Optional_. Defaults to `AE`. |
| amount | Amount in the given currency.  **Required**. |
| currency | Order currency.  Possible values: `AED`, `USD`, `QAR`, `SAR`, `EUR`, `GBP`, `BHD`.  _Optional_.  Defaults to `AED`. |

##### Response
JSON document with the following format:

    {
      'gift_json': {
          'amount': 500,
          'amount_in_currency': 500,
          'barcode_image': 'https://yougotagift.com/gifts/barcode/generate/hPc7bN/',
          'brand': 'Walmart',
          'brand_print_image': 'https://yougotagift.com/media/images/cards/print/Walmart-print-495x318.png',
          'brand_square_image': 'https://yougotagift.com/media/images/cards/fb/Walmart-FB-300x300.png',
          'brand_store_image': 'https://yougotagift.com/media/images/cards/store/Walmart-262x168.jpg',
          'country': 'US',
          'currency': 'AED',
          'expiry_date': '2017-07-31',
          'extra_fields': '',
          'gift_pdf_link': 'https://yougotagift.com/usa/gifts/95370/1BZnu0hn-qPtTg9HcVVX2keDTm71-KlOa/pdf/',
          'gift_voucher': {
                'code': 'FYAU8r',
                'url': 'http://gotagift.co/hb3Bc',
                'pin': XXX 
            },
          'id': 95370,
          'redemption_details': 'To redeem your Walmart eGift Card, please click on the Redemption URL above and enter the above Challenge Key into the website.'
        },
      'message': 'Your order has been requested successfully.',
      'purchase_request_id': 13544
    }

#### Example Request and Response

    POST /corporate/api/incentives-send/download/ HTTP/1.1
    Host: yougotagift.com
    Connection: keep-alive
    Content-Length: 155
    Authorization: Basic aW5jZW50aXZlczppbmNlbnRpdmVz
    Accept: application/json
    Accept: version=1.0

    {
       "brand" : "Virgin Megastore",                    
        "amount" : '500',                   
    }

    HTTP/1.0 200 OK
    Content-Type: application/json

    {
      'gift_json': {
          'amount': 500.0,
          'amount_in_currency': 500.0,
          'barcode_image': 'http://local.yougotagift.com:8000/gifts/barcode/generate/2752843621109/',
          'brand': 'Virgin Megastore',
          'brand_print_image': 'http://local.yougotagift.com:8000/media/images/cards/print/virgin-print_2_2.png',
          'brand_square_image': 'http://local.yougotagift.com:8000/media/images/cards/fb/virgin-196x196_2_4.jpg',
          'brand_store_image': 'http://local.yougotagift.com:8000/media/images/cards/store/virgin-262x168_2_2.jpg',
          'country': 'AE',
          'currency': 'AED',
          'expiry_date': '2017-07-31',
          'extra_fields': '',
          'gift_pdf_link': 'http://local.yougotagift.com:8000/gifts/95372/acymcSBBn3QrP4fCAz5V1ncT067JRiic/pdf/',
          'gift_voucher': {
                'code': '2752843621109'
            },
          'id': 95372,
          'redemption_details': 'This eGift Card is redeemable for any merchandise offered in any Virgin Megastore across the UAE.\r\nThis eGift Card is only valid for a one time purchase to the full value unless otherwise specified.'
        },
      'message': 'Your order has been requested successfully.',
      'purchase_request_id': 13546
    }


#### `send`
- **Endpoint** `https://yougotagift.com/corporate/api/incentives-send/send/`.
- **Returns** JSON document with the result of the request.
- **Request method**  `POST`.
- **Request format**  Should be an HTTP POST with a JSON array in the body of JSON objects for gifts requested.
- **Requires Authentication**

##### JSON Payload Parameters

| Parameter    | Description   |
| ------------ | ------------- |
| brand | Brand name **Required** (Up-to-date brand names list can be found at https://yougotagift.com/gift-card-mall/all-brands/ to access other countries lists check the end of this file) |
| country | The brand's country. Possible values: AE, LB, SA, QA, UK, US. _Optional Default AE_ |
| amount | Amount in the given currency. **Required** |
| currency | Order currency. Possible values: AED, USD, QAR, SAR, EUR, GBP, BHD _Optional Default AED_ |
| company | The company you're sending on behalf of. _Optional |
| name | The gift receiver name. **Required** |
| email | The gift receiver email. **Required** |
| phone | The gift receiver mobile phone. Should be of format +9715XXXXXXXX or 009715XXXXXXXX or 05XXXXXXXX **Required if delivering by SMS** |
| occasion  | Possible values: `birthday`, `christmas`, `teacher`, `thank you`, `congratulations`, `just because`, `wedding`, `baby`, `good luck`, `love`, `house warming`, `anniversary`, `valentine`, `mother's day`, `sorry`, `miss you`, `season's greeting`, `get well`, `encouragement`, `back to school`, `eid`, `father's day`, `graduation`, `uae national day`, `other`. _Optional_.  Defaults to `thank you`.|
| card message | Short message to put on the cover of the card.  _Optional_.  If not set, the occasion's default message will be used, i.e. "Happy Birthday" for the "birthday" occasion.|
| message | Message to put inside the card.  _Optional_. |

_Any extra parameters will also be saved in the `extra_fields` field._

##### Response
JSON document with the following format:

    {
        message: "Your order has been requested successfully."
        purchase_request_id: 13547
    }

##### Example Request and Response

    POST /corporate/api/incentives-send/send/ HTTP/1.1
    Host: yougotagift.com
    Connection: keep-alive
    Content-Length: 155
    Authorization: Basic aW5jZW50aXZlczppbmNlbnRpdmVz
    Accept: application/json
    Accept: version=1.0

    {
      "brand": "Boutique1",
      "amount": 100,
      "currency": "AED",
      "country": "AE",
      "name": "John Smith",
      "email": "john@example.com",
      "company": "Apoclapsy"
    }
  


    HTTP/1.0 200 OK
    Content-Type: application/json

    {
        "count": 1,
        "total_amount": 500,
        "message": "Your order has been requested successfully."
    }

### List of YouGotaGift.com Brands in different countries
The list of brands available in a given country can be retrieved from the following URLs.  The returned text document will be a newline-separated names of the different brands.

- UAE (AE): https://yougotagift.com/gift-card-mall/all-brands/
- Bahrain (BH): https://yougotagift.com/bahrain/gift-card-mall/all-brands/
- Lebanon (LB): https://yougotagift.com/lebanon/gift-card-mall/all-brands/
- Qatar (QA): https://yougotagift.com/qatar/gift-card-mall/all-brands/
- Saudi (SA): https://yougotagift.com/saudi/gift-card-mall/all-brands/
- USA (US): https://yougotagift.com/usa/gift-card-mall/all-brands/
- United Kingdom (UK): https://yougotagift.com/uk/gift-card-mall/all-brands/
