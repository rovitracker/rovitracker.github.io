---
layout: doc
title: "AEMP API"
section: "home"
weight: 4
---

# AEMP API

RoviTracker provides programmatic access to data through AEMP API endpoints.

## Requests

Every request must include an authorization header:

`Authorization: Bearer ACCESS_TOKEN`

replacing `ACCESS_TOKEN` with the value obtained by step 4 of the authorization process.

## API Endpoints

To request data, make an HTTP `GET` to the appropriate endpoints.

---

### AEMP 1.2

**Paginated fleet data (Fleet Details URL)**

```
GET https://aemp.rovitracker.com/api/v1/Fleet/{page}
```

**Single unit by VIN**

```
GET https://aemp.rovitracker.com/api/v1/Fleet/Equipment/{vin}
```

---

### AEMP 2.0

**Paginated fleet data (Fleet Details URL)**

```
GET https://aemp.rovitracker.com/api/v2/Fleet/{page}
```

**Single unit by equipment ID**

```
GET https://aemp.rovitracker.com/api/v2/Fleet/Equipment/{identifier}
```

**Single unit by equipment ID — date range**

```
GET https://aemp.rovitracker.com/api/v2/Fleet/Equipment/{identifier}/{startDate}/{endDate}
```

**Single unit location updates by equipment ID — date range**

```
GET https://aemp.rovitracker.com/api/v2/Fleet/Equipment/{identifier}/Location/{startDate}/{endDate}
```

---

### Path Parameters

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `{page}`       | Page number for paginated results                            |
| `{vin}`        | Vehicle Identification Number _(AEMP 1.2)_                   |
| `{identifier}` | Equipment ID (also Serial Number or VIN) _(AEMP 2.0)_        |
| `{startDate}`  | Start of the date range - ISO8601 (Ex: 2026-08-23T01:31:01Z) |
| `{endDate}`    | End of the date range - ISO8601 (Ex: 2026-08-23T01:31:01Z)   |
