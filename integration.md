---
layout: doc
title:  "Integration"
section: "home"
weight: 3
---

# Integration

RoviTracker provides programmatic access to data through AEMP API endpoints.

## Requests

Every request must include an authorization header:

`Authorization: Bearer ACCESS_TOKEN`

replacing `ACCESS_TOKEN` with the value obtained by step 4 of the authorization process.

## Endpoints

To request data, you must make an HTTP GET to: 

- https://api.rovitracker.com/v1/embed-urls

