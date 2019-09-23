---
layout: default
title: cURL
parent: Tokens
nav_order: 3
---

# cURL

## Using tokens with cURL
Tokens are OAuth 2.0 Access Tokens that allow a user to access the Kheops API server without authenticating through the Keycloak Authorization Server. 

Tokens can be used to access the Kheops DICOMweb restfull endpoint at `/api` and the wado endpoint at `/api/wado`.

There are three ways to use tokens. 

### OAuth2.0 Access Token

The prefered method is through Oauth 2.0 bearer authorization.

```console
prompt:~ user$ curl https://demo.kheops.online/api/studies -H "Authorization: Bearer B18jTXCzTrQzj1ZednqHUY"
```

### HTTP Basic Authorization

It is also possible to use the token with Basic authorization. In the case of Basic authorization, the user name provided is ignored. 

```console
prompt:~ user$ curl https://token:B18jTXCzTrQzj1ZednqHUY@demo.kheops.online/api/studies
````

### Link URL

The final stratagy to pass the token is directly through the URL. This method is the weakest from a security point of view and should be reserved for situations when compatibility is required with external applications that can not be made to work the above authorization stratagies. When passing the token in the url the `/api/link/{token}/` endpoint replaces the `/api` endpoint.

```console
prompt:~ user$ curl https://demo.kheops.online/api/link/B18jTXCzTrQzj1ZednqHUY/studies
```

## User token paramters

When using a [user token](/docs/tokens/user_tokens) all studies the user has access to will be returned. In order to specify a specific album or inbox when using a user token it is necessay specify the source using a query parameter. Use `?inbox=true` to specify the inbox. Use `?album={album_id}` to specify the album. The album ID can be found for example in the URL when displaying an album.

```console
prompt:~ user$ curl https://demo.kheops.online/api/studies?inbox=true -H "Authorization: Bearer oTK6mCWsN8RKRzlWtnK8pd"
```

```console
prompt:~ user$ curl https://demo.kheops.online/api/studies?album=dTxCc6OJyG -H "Authorization: Bearer oTK6mCWsN8RKRzlWtnK8pd"
```

