> [!NOTE]
> This API has been last updated on 2024-03-12. To know more about the changes, please refer to the [Changelog](#changelog) section.

# External API v1 Documentation

## Base URL

The API is available at the following domaine: https://api-external.app-moba.com

## Authentication

Please contact us at support@get-moba.com to get an API key

## Environments

You should be provided with two API Keys, a production one and a sandbox one. Sandbox key gives you access to sample data.

Available sample vins are the following:

- **KMHC851JFLU067181**: Timestamp `1697109652`
- **VF1AG000X63197572**: Timestamps `1697109050` and `1699790110`
- **VR3UHZKXZNT097314**: Timestamp `1696422253`
- **VF1AGVYF058697436**: Timestamp `1695895826`
- **KMHK581GFKU006734**: Timestamp `1695806157`

### Sample Request

```http
POST /v1/diagnostics/query HTTP/1.1
X-Moba-Api-Key: moba_sdbx_...
Content-Type: application/json

{
  "vins": ["VF1AG000X63197573"],
  "start": 1697109652,
  "end": 1697109656
}
```

### CURL Request

```bash
curl -X POST \
-d '{"vins": ["VF1AG000X63197572"]}' \
-H "Content-Type: application/json" \
-H "X-Moba-Api-Key: moba_sdbx_..." \
"https://api-external.app-moba.com/v1/diagnostics/query"
```

## Diagnostic URLs

Since version 1.0.1, we have introdused multi format URLs for the diagnostics. You can change template by using querystring of the output `render_url`. It can be set with key `template={template_id}`.

Available formats are the following:

- PDF: Usual PDF certificate. It is the most detailed document.
  - ID: `pdf`
  - Url: `https://certificate.get-moba.com/certificates/{token}.pdf`
  - Alias: `https://certificate.get-moba.com/miner/api.php?action=diagFromToken&token={token}&template=pdf`
- JPG (800 x 600): Image of the certificate. It is the most compact document and can be embedded on webpages as so.
  - ID: `jpg_800_600`
  - Url: `https://certificate.get-moba.com/miner/api.php?action=diagFromToken&token={token}&template=jpg_800_600`

By default, it is the template set for your company that will be served. If you want to change this default template, you can contact us.

> [!NOTE]
> Default template is also the one used when downloading the certificate from the app by the operator.

## General Response JSON

- `datetime` (string): Datetime of the response.
- `<object>` (array): Array of objects depending of the endpoint
- `many` (boolean): Whether the response contains multiple diagnostics objects.
- `n_results` (int): Number of diagnostics objects in the response.
- `status` (int): HTTP status code of the response.

## Authentication Header

- `X-Moba-Api-Key`: API Key for authentication.


## Query Diagnostics

### Endpoint

- **URL**: `/v1/diagnostics/query`
- **Method**: `POST`

### JSON Payload

- `vins` (array): Array of VINs for which to query diagnostics (Max. 10)
- `start` (int): Start timestamp for filtering diagnostics (optional).
- `end` (int): End timestamp for filtering diagnostics (optional).

### Diagnostics Object

- `c0` (float): Battery capacity in unit set with `c0_unit`.
- `c0_unit` (string): Unit of the battery capacity.
- `issue_date` (string): Date of the diagnostic.
- `mileage` (int): Mileage of the vehicle.
- `name` (string): Name of the vehicle.
- `pdf_token` (string): Token of the PDF certificate.
- `pdf_url` (string): URL of the PDF certificate.
- `render_url` (string): URL of the certificate rendering (Will use default template).
- `download_url` (string): URL of the certificate download (Will use default template).
- `ranges` (object): Object containing the ranges of the vehicle.
- `soc` (float): State of charge of the vehicle.
- `soh` (float): State of health of the vehicle.
- `range_unit` (string): Unit of the ranges.
- `vin` (string): VIN of the vehicle.

### Sample Request

```http
POST /v1/diagnostics/query HTTP/1.1
X-Moba-Api-Key: moba_...
Content-Type: application/json

{
  "vins": ["VF1AG000X63197573"],
  "start": 1697109652,
  "end": 1697109656
}
```

### Sample Response

```json
{
  "datetime": "2023-11-17T14:50:47.042441",
  "diagnostics": [
    {
      "c0": 52,
      "c0_unit": "kWh",
      "issue_date": "2023-10-12T13:10:50",
      "mileage": 45785,
      "name": "Renault Zoé 50 kWh R110",
      "pdf_token": "c927fea2fad",
      "pdf_url": "https://certificate.get-moba.com/certificates/c927fea2fad.pdf",
      "render_url": "https://certificate.get-moba.com/miner/api.php?action=diagFromToken&token=c927fea2fad",
      "download_url": "https://certificate.get-moba.com/miner/api.php?action=diagFromToken&token=c927fea2fad&download=1",
      "ranges": {
        "highway_cycle_summer": "263 - 291 km",
        "highway_cycle_winter": "216 - 239 km",
        "mixed_cycle_summer": "312 - 345 km",
        "mixed_cycle_winter": "238 - 263 km",
        "urban_cycle_summer": "216 - 239 km",
        "urban_cycle_winter": "186 - 206 km"
      },
      "soc": 87.5,
      "soh": 87,
      "range_unit": "km",
      "vin": "VF1AG000X63197573"
    }
  ],
  "many": true,
  "n_results": 1,
  "status": 200
}
```

## Get Diagnostic by Token

### Endpoint

- **URL**: `/v1/diagnostics/<token>`
- **Method**: `GET`

### Diagnostic Object

(See [Diagnostics Object](#diagnostics-object))

### Sample Request

```http
GET /v1/diagnostics/c927fea2fad HTTP/1.1
X-Moba-Api-Key: moba_...
```

### Sample Response

```json
{
  "datetime": "2023-11-17T14:50:47.042441",
  "diagnostic": {
      "c0": 52,
      "c0_unit": "kWh",
      "issue_date": "2023-10-12T13:10:50",
      "mileage": 45785,
      "name": "Renault Zoé 50 kWh R110",
      "pdf_token": "c927fea2fad",
      "pdf_url": "https://certificate.get-moba.com/certificates/c927fea2fad.pdf",
      "render_url": "https://certificate.get-moba.com/miner/api.php?action=diagFromToken&token=c927fea2fad",
      "download_url": "https://certificate.get-moba.com/miner/api.php?action=diagFromToken&token=c927fea2fad&download=1",
      "ranges": {
        "highway_cycle_summer": "263 - 291 km",
        "highway_cycle_winter": "216 - 239 km",
        "mixed_cycle_summer": "312 - 345 km",
        "mixed_cycle_winter": "238 - 263 km",
        "urban_cycle_summer": "216 - 239 km",
        "urban_cycle_winter": "186 - 206 km"
      },
      "soc": 87.5,
      "soh": 87,
      "range_unit": "km",
      "vin": "VF1AG000X63197573"
    },
  "status": 200
}
```

## Webhook

Using webhooks allow your company to get real time updates on the diagnostics of your vehicles. You can use them to trigger actions in your systems, like sending an email, updating a database, or sending a message to a chat application.

If you want to access webhooks, you need to contact us at contact@get-moba.com.

### Webhook Signature

Along with the payload, a signature is sent in the HTTP headers of the request under the key `X-Moba-Signature`. This signature is a HMAC-SHA256 hash of the payload using the secret key of the webhook as the key.

To verify the signature, you can use the following code:

#### Python

```python
import hmac
import hashlib

def verify_signature(payload, signature, secret):
    return hmac.compare_digest(
        signature,
        hmac.new(
            secret.encode('utf-8'),
            payload.encode('utf-8'),
            hashlib.sha256
        ).hexdigest()
    )
```

#### PHP

```PHP
function verify_signature($payload, $signature, $secret) {
    return hash_equals(
        $signature,
        hash_hmac('sha256', $payload, $secret)
    );
}
```

#### JavaScript

```javascript
const crypto = require('crypto');

function verifySignature(payload, signature, secret) {
    return crypto.createHmac('sha256', secret).update(payload).digest('hex') === signature;
}
```

To get the secret of your webhook, you can query `/webhooks` service of the API.

### Webhook Payload

The payload sent to the webhook URL is a JSON object containing the following keys:
- `datetime` (string): Datetime of the event.
- `event` (string): Event that triggered the webhook.
- `data` (object): Object containing the data of the event.
    - `date` (string): Date of the diagnostic.
    - `vin` (string): VIN of the vehicle.
    - `soc` (float): State of charge of the vehicle.
    - `soh` (float): State of health of the vehicle.
    - `mileage` (float): Mileage of the vehicle.
    - `token` (string): Token of the diagnostic.
    - `more_info_api_url` (string): URL of the API to get more information about the diagnostic.
- `test` (boolean): Will exist and be true when testing Webhook with the API

### Example Payload

```
{
  "datetime": "2024-03-08 15:27:50",
  "event": "diag_done",
  "data": {
    "date": "2024-03-06 17:30:45",
    "vin": "VF1AG000X63197573",
    "soc": 97.3,
    "soh": 74,
    "mileage": 22453,
    "token": "c12345abc",
    "more_info_api_url": "https://api-external.app-moba.com/v1/diagnostics/c12345abc"
  }
}
```

### Responsing to a Webhook

When a webhook is called, the server should respond with a status code of 200 if the payload was received and processed successfully. If the server responds with a status code other than 200, the webhook will be retried later.
The webhook server has *up to 10 seconds* to respond to the webhook call.

### Webhook Retries

When a webhook fails to be called, it will be retried up to 9 times. After the last failure, the webhook will be disabled and will need to be re-enabled manually.

Retries happend at the following intervals:
- 1 minute
- 5 minutes
- 15 minutes
- 1 hour
- 4 hours
- 1 day
- 2 days
- 4 days
- 1 week



## List Webhooks

Lists all webhooks for the authenticated API Key.

### Endpoint

- **URL**: `/v1/webhooks`
- **Method**: `GET`

### Webhook Object

- `id` (int): ID of the webhook.
- `name` (string): Name of the webhook.
- `url` (string): URL of the webhook.
- `active` (boolean): Whether the webhook is active.
- `created_at` (string): Date of creation of the webhook.
- `entity_id` (int): ID of the entity linked to the webhook.
- `event` (string): Event that triggers the webhook.
- `last_used` (string): Date of the last use of the webhook.
- `level` (string) Define at which level the webhook triggers (Can be `entity`, `site`, `team` or `user`)
- `level_id` (int): ID of the level linked to the webhook.
- `n_retries` (int): Max number of retries the webhook will do before being disabled.
- `secret` (string): Secret key of the webhook. This secret will be sent in the HTTP headers of the request under the key `X-Moba-Secret`
- `updated_at` (string): Date of the last update of the webhook.


### Sample Request

```http
GET /v1/webhooks HTTP/1.1
X-Moba-Api-Key: moba_...
```

### Sample Response

```json
{
  "datetime": "2023-11-17T14:50:47.042441",
  "webhooks": [
    {
      "active": true,
      "created_at": "2024-03-08 10:53:27",
      "entity_id": 1234,
      "event": "diag_done",
      "id": 123,
      "last_used": "2024-03-08 15:29:09",
      "level": "entity",
      "level_id": 1,
      "n_retries": 3,
      "name": "Webhook Name",
      "secret": "123456789abcdef",
      "updated_at": "2024-03-08 15:29:09",
      "url": "https://my-webhook-url.com"
    }
  ],
  "many": true,
  "n_results": 1,
  "status": 200
}
```

## Test Webhook

Tests a webhook by sending a test payload to the URL.

### Endpoint

- **URL**: `/v1/webhooks/<webhook_id>/test`
- **Method**: `GET`

### Querystring

- `token` (string): Optional: Test diagnostic token to be sent in the request. This diagnostic token has to exist. If not provided, test data will be used and sent to your webhook.

### Output

- `server_response` (object): Object containing the response of the server.

### Sample Request

```http
GET /v1/webhooks/123/test?token=c927fea2fad HTTP/1.1
```

### Sample Response

```json
{
  "datetime": "2023-11-17T14:50:47.042441",
  "server_response": {
    "status": 200,
    "message": "Webhook called successfully"
  },
  "status": 200
}
```

## Changelog

### v1.0.0 - 2023-11-17

- Initial release

### v1-0-1 - 2024-01-16

- Added properties `render_url` and `download_url` to `diagnostics` objects

### v1-0-2 - 2024-03-13

- Added `test` key to the payload of the webhook
- Added webhook integration