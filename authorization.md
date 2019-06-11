---
layout: doc
title:  Authorization
section: home
weight: 2
---

# Authorization

Authorization is done using the OAuth2 standard.

## Definitions

RoviTracker (server)
: RoviTracker web application that hosts all of the equipment info and metric data.
  
Application
: Rental or management software (other than RoviTracker) accessing protected resources hosted by RoviTracker, on 
    behalf or a resource owner (shared customer).
    
Customer
: The user (shared customer) who authorizes an application to access their account. 


## Intended Use

A `Customer` wants to see their equipment data collected by `RoviTracker` displayed on the `Application` 
(rental management software, etc).

The `Customer` authorizes the `Application` to access their `RoviTracker` data.

The `Application` requests data regularly from `RoviTracker` using the authorization provided by the `Customer`.

## Registration Process

The registration process only happens once at the beginning:

1. Register `Application` with RoviTracker by providing a company name and logo.
2. Receive a `client_id` and a `client_secret`.

## Authorization (preferred: automatic, customer directed)

1. `Application` directs `Customer` to https://rovitracker.com/app/oauth2/authorize?client_id=CLIENT_ID&state=STATE&redirect_uri=REDIRECT_URI
    With variables replaced with:
    * `CLIENT_ID` - `client_id` value obtained through the registration process
    * `STATE` - A value (nonce) used to maintain state between `RoviTracker` server and `Application`: https://tools.ietf.org/html/rfc6749#section-4.1.1
    * `REDIRECT_URI` - A URL controlled by `Application` where `RoviTracker` will send the `Customer` after authorization.
    This URL will prompt the `Customer` to authorize `Application` to access their data stored at `RoviTracker`.
2. `Customer` confirms authorization prompt
3. `Customer` is redirected to REDIRECT_URI?code=AUTHORIZATION_CODE&state=STATE 
    Where:
    * `REDIRECT_URI` is the URL provided by the `Application` in step 1.
    * `STATE` will be the same value provided by the `Application` in step 1.
4. `Application` takes the `AUTHORIZATION_CODE` from the query string parameter and exchanges it for an `access_token` by making an HTTP
    POST request to: https://rovitracker.com/app/oauth2/token with body parameters of: 
    * `client_id`: Value obtained through registration process.
    * `client_secret`: Value obtrained through registration process.
    * `code`: Authorization code obtained through redirect (step 3).
    The server will respond with JSON response with an access token like:
      ```{"access_token": "234acb234badeeefee2342", "type": "Bearer"}```
5. `Application` stores and protects the access token for later use when requesting data.

## Authorization (alternate: manual, RoviTracker-directed)

1. `Application` sends an email to `RoviTracker` requesting access to a particular client. 
2. `RoviTracker` confirms with `Customer` that the request is valid.
3. `RoviTracker` sends an email with `access_token` to `Application`.


## Requesting Data

You can now request data using the [AEMP API](/aemp_api.html)

