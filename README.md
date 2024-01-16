> [!NOTE]
> This API has been last updated on 2024-01-16. To know more about the changes, please refer to the [Changelog](#changelog) section.

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

## Query Diagnostics

### Endpoint

- **URL**: `/v1/diagnostics/query`
- **Method**: `POST`

### JSON Payload

- `vins` (array): Array of VINs for which to query diagnostics (Max. 10)
- `start` (int): Start timestamp for filtering diagnostics (optional).
- `end` (int): End timestamp for filtering diagnostics (optional).

### Headers

- `X-Moba-Api-Key`: API Key for authentication.

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

## Changelog

### 2023-11-17

- Initial release

### 2024-01-16

- Added properties `render_url` and `download_url` to `diagnostics` objects
