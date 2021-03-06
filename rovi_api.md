---
layout: doc
title:  "Rovi API"
section: "home"
weight: 3
---

# Integration

RoviTracker provides programmatic access to data through REST/JSON endpoints.

## Requests

Every request must include an authorization header:

`Authorization: Bearer ACCESS_TOKEN`

replacing `ACCESS_TOKEN` with the value obtained by step 4 of the authorization process.

## Endpoints

To request data, you must make an HTTP GET one of the available endpoins. 
All requests should be made using this following prefix:
```
https://api.rovitracker.com/v1
```


### `GET /branch/current/urls`

Get URLs for the branch. Only contains the `maptracEmbed` key at the moment. URLs in the response will create a embed session on the user's browser lasting 1 hour.

#### Response
JSON object with the following keys:
- `maptracEmbed`: Embeddable URL showing all of the branch's assets in a map.


#### Sample 

```curl
curl -X GET \
  https://api.rovitracker.com/v1/branch/current/urls \
  -H 'Authorization: Bearer c3a56f16464a9c16dde83fca462c346f' 
```
```json
{
	"maptracEmbed": "https://app.rovitracker.com/app/embed/auth?req=%2Fapp%2Fembed%2Fmaptrac&jws=eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiI2NyIsImV4cCI6MTU2MTU3OTUxM30.CqZlLWqwppyk5pbD1OKjUgYerqsvKzr2DPSEbZMMA58"
}
```

### `GET /branch/current/batch/assetUrls`

Get URLs that only show data for a subset of assets. URLs in the response will create a embed session on the user's browser lasting 1 hour.

#### Query Parameters
- `assets`: Comma-delimited list of asset ids. Ex: `thing:34,thing:32,thing:4`

#### Response
JSON object with the following keys:
- `maptracEmbed`: Embeddable URL showing only the requested assets in a map.

#### Sample
```curl
curl -X GET \
  'https://api.rovitracker.com/v1/branch/current/batch/assetUrls?assets=thing:1,thing:2,thing:3' \
  -H 'Authorization: Bearer c3a56f16464a9c16dde83fca462c346f'
```
```json
{
	"maptracEmbed": "https://app.rovitracker.com/app/embed/auth?req=%2Fapp%2Fembed%2Fmaptrac%23assets%3Dthing%3A1%2Cthing%3A2%2Cthing%3A3&jws=eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiI2NyIsImV4cCI6MTU2MTU3OTgzMn0.jMheVYH87CHxUBPbWhgfv7DiNXAY3xxzFD_S-khsyTE"
}
```

### `GET /branch/current/assets` 

Return a list of assets belonging to the branch.

#### Response
JSON array where each element has the following keys:
- `assetId : string` - RoviTracker ID of the asset
- `type : "thing"|"area"` - Asset type
- `name : string` - Name given to the asset by the customer
- `serialNumber : string` - Only available on assets of type `thing`.
- `category : string` - Customer-defined category that the asset belongs to. Only available on assets of type `thing`
- `urls : json object` - JSON object with entries for URLs pointing to this asset.
- `metrics : (object | null)`: JSON object with entries for each of the most recent metrics collected for the equipment. `null` if no metrics have been collected.

The `metrics` object will have entries following this pattern:
```json
  {
    $metricName: {
      value: $metricValue,
      meta: {
        collectedOn: $timestamp,
        ... others
      }
    },
    ...
  }
}
```

It can have any combination of the following metrics:
- `fuel` [`number`, 0 to 100] - Amount of fuel as a percentage
- `ignition` [`boolean`] - `true` if ignition is on
- `gps` [`{"lat":number,"lng":number}`] - Last know position
- `engine-runtime` [`number`, milliseconds]
- `light1-runtime` [`number`, milliseconds]
- `odometer` [`number`, centimeters]
- ... others

#### Sample

```
curl -X GET \
  https://api.rovitracker.com/v1/branch/current/assets \
  -H 'Authorization: Bearer c3a56f16464a9c16dde83fca462c346f'
```
```json
[
  {
    "assetId":"thing:1138",
    "type":"thing",
    "name":"Full Mobile Demo Display",
    "serialNumber":"",
    "category":"Display Demos",
    "urls":{
      "maptrackEmbed":"https://app.rovitracker.com/app/embed/auth?req=%2Fapp%2Fembed%2Fmaptrac%23focus%3Dthing%3A1138&jws=eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiI2NyIsImV4cCI6MTU2MTU4MjE2NH0.3EhI09gLfdX9eFvFwLoAz-NP5Bh2Ikax_88R6yb2oWo"
    },
    "metrics":null
  },
  {
    "assetId":"thing:1161",
    "type":"thing",
    "name":"CAN1200 Morey",
    "serialNumber":"564",
    "category":"Company Vehicle",
    "urls":{
      "maptrackEmbed":"https://app.rovitracker.com/app/embed/auth?req=%2Fapp%2Fembed%2Fmaptrac%23focus%3Dthing%3A1161&jws=eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiI2NyIsImV4cCI6MTU2MTU4MjE2NH0.3EhI09gLfdX9eFvFwLoAz-NP5Bh2Ikax_88R6yb2oWo"
    },
    "metrics":{
      "last-device-update":{
        "value":"2018-12-17T19:09:39Z",
        "meta":{
          "collectedOn":"2018-12-17T19:09:39Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "time-zone":{
        "value":"America/Denver",
        "meta":{
          "collectedOn":"2018-12-17T19:09:40.847Z",
          "source":"tzGeocoder",
          "other":{

          }
        }
      },
      "fuel":{
        "value":64,
        "meta":{
          "collectedOn":"2018-03-29T14:23:55Z",
          "source":"morey-Vehicle_3",
          "other":{

          }
        }
      },
      "altitude":{
        "value":139954,
        "meta":{
          "collectedOn":"2018-12-17T19:09:39Z",
          "source":"calamp - altitude",
          "other":{

          }
        }
      },
      "cab-temperature":{
        "value":999,
        "meta":{
          "collectedOn":"2018-09-26T22:35:32Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "solar-battery1":{
        "value":4184,
        "meta":{
          "collectedOn":"2018-09-26T22:35:32Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "address-region":{
        "value":"UT",
        "meta":{
          "collectedOn":"2018-12-17T19:09:40.847Z",
          "source":"usStatesGeocoder",
          "other":{

          }
        }
      },
      "speed":{
        "value":125,
        "meta":{
          "collectedOn":"2018-12-17T19:09:39Z",
          "source":"calamp - speed",
          "other":{

          }
        }
      },
      "in-motion":{
        "value":true,
        "meta":{
          "collectedOn":"2018-03-29T14:35:24Z",
          "source":"morey-Ign_off",
          "other":{

          }
        }
      },
      "ignition":{
        "value":false,
        "meta":{
          "collectedOn":"2018-12-17T19:09:39Z",
          "source":"calamp - digital-input",
          "other":{

          }
        }
      },
      "lights1-runtime":{
        "value":0,
        "meta":{
          "collectedOn":"2018-09-26T22:35:32Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "battery":{
        "value":0,
        "meta":{
          "collectedOn":"2018-12-17T19:09:39Z",
          "source":"calamp - accum-obd-battery[9]",
          "other":{

          }
        }
      },
      "heading":{
        "value":115,
        "meta":{
          "collectedOn":"2018-12-17T19:09:39Z",
          "source":"calamp - heading",
          "other":{

          }
        }
      },
      "lights2-runtime":{
        "value":0,
        "meta":{
          "collectedOn":"2018-09-26T22:35:32Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "odometer":{
        "value":0,
        "meta":{
          "collectedOn":"2018-12-17T19:09:39Z",
          "source":"calamp - accum-obd-odometer",
          "other":{

          }
        }
      },
      "gps":{
        "value":{
          "lat":40.618971599999995,
          "lng":-111.9992915
        },
        "meta":{
          "collectedOn":"2018-12-17T19:09:39Z",
          "source":"calamp - gps",
          "other":{

          }
        }
      },
      "engine-runtime":{
        "value":0,
        "meta":{
          "collectedOn":"2018-12-17T19:09:39Z",
          "source":"calamp - accum-runtime[8] offset: PT0S",
          "other":{

          }
        }
      },
      "signal-strength":{
        "value":4,
        "meta":{
          "collectedOn":"2018-12-17T19:09:39Z",
          "source":"calamp - rssi-signal-strength",
          "other":{

          }
        }
      },
      "device-battery":{
        "value":1257,
        "meta":{
          "collectedOn":"2018-03-29T14:35:24Z",
          "source":"morey-Ign_off",
          "other":{

          }
        }
      }
    }
  },
  {
    "assetId":"thing:1162",
    "type":"thing",
    "name":"1638 Cam Tower",
    "serialNumber":"2241128176",
    "category":"Light Towers",
    "urls":{
      "maptrackEmbed":"https://app.rovitracker.com/app/embed/auth?req=%2Fapp%2Fembed%2Fmaptrac%23focus%3Dthing%3A1162&jws=eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiI2NyIsImV4cCI6MTU2MTU4MjE2NH0.3EhI09gLfdX9eFvFwLoAz-NP5Bh2Ikax_88R6yb2oWo"
    },
    "metrics":{
      "lights4-runtime":{
        "value":142000,
        "meta":{
          "collectedOn":"2018-10-15T18:33:35Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "ip-endpoint":{
        "value":"174.208.20.176:5669",
        "meta":{
          "collectedOn":"2018-10-15T18:33:35Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "input1":{
        "value":false,
        "meta":{
          "collectedOn":"2018-10-15T18:33:35Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "input3":{
        "value":false,
        "meta":{
          "collectedOn":"2018-10-15T18:33:35Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "time-zone":{
        "value":"America/Denver",
        "meta":{
          "collectedOn":"2018-10-15T18:33:36.609Z",
          "source":"tzGeocoder",
          "other":{

          }
        }
      },
      "current1":{
        "value":0,
        "meta":{
          "collectedOn":"2018-10-15T18:33:35Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "altitude":{
        "value":0,
        "meta":{
          "collectedOn":"2018-10-15T18:33:35Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "cab-temperature":{
        "value":0,
        "meta":{
          "collectedOn":"2018-10-15T18:33:35Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "lights3-runtime":{
        "value":142000,
        "meta":{
          "collectedOn":"2018-10-15T18:33:35Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "input2":{
        "value":false,
        "meta":{
          "collectedOn":"2018-10-15T18:33:35Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "address-region":{
        "value":"UT",
        "meta":{
          "collectedOn":"2018-10-15T18:33:36.609Z",
          "source":"usStatesGeocoder",
          "other":{

          }
        }
      },
      "speed":{
        "value":0,
        "meta":{
          "collectedOn":"2018-10-15T18:33:35Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "ignition":{
        "value":false,
        "meta":{
          "collectedOn":"2018-10-15T18:33:35Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "lights1-runtime":{
        "value":437000,
        "meta":{
          "collectedOn":"2018-10-15T18:33:35Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "battery":{
        "value":23658,
        "meta":{
          "collectedOn":"2018-10-15T18:33:35Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "heading":{
        "value":0,
        "meta":{
          "collectedOn":"2018-10-15T18:33:35Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "lights2-runtime":{
        "value":142000,
        "meta":{
          "collectedOn":"2018-10-15T18:33:35Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "gps":{
        "value":{
          "lat":40.6187058,
          "lng":-111.9993068
        },
        "meta":{
          "collectedOn":"2018-10-15T18:33:35Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "geo-regions":{
        "value":[
          "region:UT"
        ],
        "meta":{
          "collectedOn":"2018-10-15T18:33:36.663Z",
          "source":"enhanceEvent",
          "other":{

          }
        }
      },
      "engine-runtime":{
        "value":142000,
        "meta":{
          "collectedOn":"2018-10-15T18:33:35Z",
          "source":"calamp",
          "other":{

          }
        }
      },
      "input4":{
        "value":false,
        "meta":{
          "collectedOn":"2018-10-15T18:33:35Z",
          "source":"calamp",
          "other":{

          }
        }
      }
    }
  }
]
```
