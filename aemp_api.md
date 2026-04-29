---
layout: doc
title:  "AEMP API"
section: "home"
weight: 4
---

# AEMP API

RoviTracker provides programmatic access to data through AEMP API endpoints.

## Requests

Every request must include an authorization header:

`Authorization: Bearer ACCESS_TOKEN`

replacing `ACCESS_TOKEN` with the value obtained by step 4 of the authorization process.

## AEMP API Endpoints

To request data, make an HTTP `GET` to the appropriate endpoints.

---

## AEMP 1.2

### Endpoints

**Paginated fleet data**
```
GET https://aemp.rovitracker.com/api/Fleet/{page}
```

**Single unit by VIN**
```
GET https://aemp.rovitracker.com/api/Fleet/{vin}
```

**Single unit by VIN — date range**
```
GET https://aemp.rovitracker.com/api/Fleet/{vin}/{startDate}/{endDate}
```

---

## AEMP 2.0

### Endpoints

**Paginated fleet data**
```
GET https://aemp.rovitracker.com/api/v2/Fleet/{page}
```

**Single unit by equipment ID**
```
GET https://aemp.rovitracker.com/api/v2/Fleet/{equipmentID}
```

**Single unit by equipment ID — date range**
```
GET https://aemp.rovitracker.com/api/v2/Fleet/{equipmentID}/{startDate}/{endDate}
```

---

## Path Parameters

| Parameter | Description |
|---|---|
| `{page}` | Page number for paginated results |
| `{vin}` | Vehicle Identification Number *(AEMP 1.2)* |
| `{equipmentID}` | Unique equipment identifier *(AEMP 2.0)* |
| `{startDate}` | Start of the date range |
| `{endDate}` | End of the date range |


