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

## Endpoints

To request data, you must make an HTTP GET to: 

- https://aemp.rovitracker.com/api/Fleet/{page}
- https://aemp.rovitracker.com/api/Fleet/{vin}
- https://aemp.rovitracker.com/api/Fleet/{vin}/{startDate}/{endDate}


