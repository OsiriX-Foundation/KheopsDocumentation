---
layout: default
title: cURL
parent: Tokens
nav_order: 3
permalink: /docs/tokens/curl
---

# cURL
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Using tokens with cURL
Tokens are OAuth 2.0 Access Tokens that allow a user to access the Kheops API server without authenticating through the Keycloak Authorization Server. 

Tokens can be used to access the Kheops DICOMweb restfull endpoint at `/api` and the wado-uri endpoint at `/api/wado`.

There are three ways to use tokens. 

### OAuth2.0 Access Token

The prefered method is through OAuth 2.0 Bearer Authorization.

```console
prompt:~ user$ curl https://demo.kheops.online/api/studies -H "Authorization: Bearer B18jTXCzTrQzj1ZednqHUY"
```

### HTTP Basic Authorization

It is also possible to use the token with HTTP Basic Authorization. In the case of Basic authorization, the user name provided is ignored. 

```console
prompt:~ user$ curl https://token:B18jTXCzTrQzj1ZednqHUY@demo.kheops.online/api/studies
````

### Link URL

The final strategy to pass the token is directly through the URL. This method is the weakest from a security point of view and should be reserved for situations when compatibility is required with external applications that can not be made to work using the above authorization stratagies. When passing the token in the url the `/api/link/{token}/` endpoint replaces the `/api` endpoint.

```console
prompt:~ user$ curl https://demo.kheops.online/api/link/B18jTXCzTrQzj1ZednqHUY/studies
```

---

## Example DICOMweb Requests

Below are sample DICOMweb requests. For more information refer to [Part 18 (DICOMweb) of the DICOM standard](http://dicom.nema.org/medical/dicom/current/output/html/part18.html).

### Get a list of studies

```console
prompt:~ user$ curl https://demo.kheops.online/api/studies -H "Authorization: Bearer B18jTXCzTrQzj1ZednqHUY"
```

```json
[{"00080020":{"vr":"DA","Value":["20080729"]},"00080030":{"vr":"TM","Value":["125554.000000"]},"00080050":{"vr":"SH","Value":["A10003204947"]},"00080056":{"vr":"CS","Value":["ONLINE"]},"00080061":{"vr":"CS","Value":["CT"]},"00080090":{"vr":"PN","Value":[{"Alphabetic":"HAUSER^HERMANN"}]},"00100010":{"vr":"PN","Value":[{"Alphabetic":"CARDIX"}]},"00100020":{"vr":"LO","Value":["222111"]},"00100030":{"vr":"DA","Value":["20080822"]},"00100040":{"vr":"CS","Value":["0000"]},"0020000D":{"vr":"UI","Value":["2.16.840.1.113669.632.20.121711.10000158860"]},"00200010":{"vr":"SH","Value":["333111"]},"00201206":{"vr":"IS","Value":[1]},"00201208":{"vr":"IS","Value":[1356]}},{"00080020":{"vr":"DA","Value":["20061019"]},"00080030":{"vr":"TM","Value":["143739.921000"]},"00080050":{"vr":"SH","Value":["0"]},"00080056":{"vr":"CS","Value":["ONLINE"]},"00080061":{"vr":"CS","Value":["CT"]},"00080090":{"vr":"PN","Value":[{"Alphabetic":"dAEvNszxOzfg"}]},"00100010":{"vr":"PN","Value":[{"Alphabetic":"MAGIX"}]},"00100020":{"vr":"LO","Value":["F063TE"]},"00100030":{"vr":"DA","Value":["NULL"]},"00100040":{"vr":"CS","Value":["NULL"]},"0020000D":{"vr":"UI","Value":["1.3.6.1.4.1.19291.2.1.1.1128230802910216257358211407773"]},"00200010":{"vr":"SH","Value":["A10026587077"]},"00201206":{"vr":"IS","Value":[10]},"00201208":{"vr":"IS","Value":[760]}}]
```

### Download a study as a DICOM Zip file

```console
prompt:~ user$ curl https://demo.kheops.online/api/studies/1.3.6.1.4.1.19291.2.1.1.1128230802910216257358211407773 -H "Authorization: Bearer B18jTXCzTrQzj1ZednqHUY" -H "Accept: application/zip" -OJ
```

Or pass the everything in the URL **(much less secure)**

```console
prompt:~ user$ curl https://demo.kheops.online/api/link/B18jTXCzTrQzj1ZednqHUY/studies/1.3.6.1.4.1.19291.2.1.1.1128230802910216257358211407773?accept=application%2F/zip -OJ
```

### Get a list of series in a study

```console
prompt:~ user$ curl https://demo.kheops.online/api/studies/1.3.6.1.4.1.19291.2.1.1.1128230802910216257358211407773/series -H "Authorization: Bearer B18jTXCzTrQzj1ZednqHUY"
```

```json
[{"00080005":{"vr":"CS","Value":["ISO_IR 100"]},"00080054":{"vr":"AE","Value":["DCM4CHEE"]},"00080056":{"vr":"CS","Value":["ONLINE"]},"00080060":{"vr":"CS","Value":["CT"]},"0008103E":{"vr":"LO","Value":["Cir  CardiacCirc  3.0  B20f  0-90% RETARD_DECLECHEMENT 50 %"]},"00081190":{"vr":"UR","Value":["https://demo.kheops.online/api/studies/1.3.6.1.4.1.19291.2.1.1.1128230802910216257358211407773/series/1.3.6.1.4.1.19291.2.1.2.11282308029102162573582122151467"]},"0020000D":{"vr":"UI","Value":["1.3.6.1.4.1.19291.2.1.1.1128230802910216257358211407773"]},"0020000E":{"vr":"UI","Value":["1.3.6.1.4.1.19291.2.1.2.11282308029102162573582122151467"]},"00200011":{"vr":"IS","Value":[10]},"00201209":{"vr":"IS","Value":[76]},"00400275":{"vr":"SQ","Value":[{"00400007":{"vr":"LO","Value":["CT1 cardiaque"]},"00400008":{"vr":"SQ","Value":[{"00080100":{"vr":"SH","Value":["CTCOEUR"]},"00080102":{"vr":"SH","Value":["XPLORE"]},"00080104":{"vr":"LO","Value":["CT1 cardiaque"]}}]},"00400009":{"vr":"SH","Value":["A10026587078"]},"00401001":{"vr":"SH","Value":["A10026587077"]}}]}},{"00080005":{"vr":"CS","Value":["ISO_IR 100"]},"00080054":{"vr":"AE","Value":["DCM4CHEE"]},"00080056":{"vr":"CS","Value":["ONLINE"]},"00080060":{"vr":"CS","Value":["CT"]},"0008103E":{"vr":"LO","Value":["Cir  CardiacCirc  3.0  B20f  0-90% RETARD_DECLECHEMENT 30 %"]},"00081190":{"vr":"UR","Value":["https://demo.kheops.online/api/studies/1.3.6.1.4.1.19291.2.1.1.1128230802910216257358211407773/series/1.3.6.1.4.1.19291.2.1.2.11282308029102162573582118671159"]},"0020000D":{"vr":"UI","Value":["1.3.6.1.4.1.19291.2.1.1.1128230802910216257358211407773"]},"0020000E":{"vr":"UI","Value":["1.3.6.1.4.1.19291.2.1.2.11282308029102162573582118671159"]},"00200011":{"vr":"IS","Value":[10]},"00201209":{"vr":"IS","Value":[76]},"00400275":{"vr":"SQ","Value":[{"00400007":{"vr":"LO","Value":["CT1 cardiaque"]},"00400008":{"vr":"SQ","Value":[{"00080100":{"vr":"SH","Value":["CTCOEUR"]},"00080102":{"vr":"SH","Value":["XPLORE"]},"00080104":{"vr":"LO","Value":["CT1 cardiaque"]}}]}]
```

### Download a thumbnail of a series

```console
prompt:~ user$ curl "https://demo.kheops.online/api/wado?studyUID=1.3.6.1.4.1.19291.2.1.1.1128230802910216257358211407773&seriesUID=1.3.6.1.4.1.19291.2.1.2.11282308029102162573582122151467&requestType=WADO&rows=250&columns=250&contentType=image%2Fjpeg" -H "Authorization: Bearer B18jTXCzTrQzj1ZednqHUY"
```

![Series Thumbnail](https://demo.kheops.online/api/link/B18jTXCzTrQzj1ZednqHUY/wado?studyUID=1.3.6.1.4.1.19291.2.1.1.1128230802910216257358211407773&seriesUID=1.3.6.1.4.1.19291.2.1.2.11282308029102162573582122151467&requestType=WADO&rows=250&columns=250&contentType=image%2Fjpeg)

### Download an image of a specific DICOM instance.

```console
prompt:~ user$ curl "https://demo.kheops.online/api/wado?studyUID=1.3.6.1.4.1.19291.2.1.1.1128230802910216257358211407773&seriesUID=1.3.6.1.4.1.19291.2.1.2.11282308029102162573582122151467&objectUID=1.3.6.1.4.1.19291.2.1.3.11282308029102162573582122291522&requestType=WADO&rows=250&columns=250&contentType=image%2Fjpeg" -H "Authorization: Bearer B18jTXCzTrQzj1ZednqHUY"
```


![Series Thumbnail](https://demo.kheops.online/api/link/B18jTXCzTrQzj1ZednqHUY/wado?studyUID=1.3.6.1.4.1.19291.2.1.1.1128230802910216257358211407773&seriesUID=1.3.6.1.4.1.19291.2.1.2.11282308029102162573582122151467&objectUID=1.3.6.1.4.1.19291.2.1.3.11282308029102162573582122291522&requestType=WADO&rows=250&columns=250&contentType=image%2Fjpeg)

---

## User token parameters

When using a [user token](/docs/tokens/user_tokens) all studies the user has access to will be returned. In order to specify a specific album or inbox when using a user token it is necessay specify the source using a query parameter. Use `?inbox=true` to specify the inbox. Use `?album={album_id}` to specify the album. The album ID can be found in the URL when viewing and album in the Kheops webUI.

```console
prompt:~ user$ curl https://demo.kheops.online/api/studies?inbox=true -H "Authorization: Bearer oTK6mCWsN8RKRzlWtnK8pd"
```

```console
prompt:~ user$ curl https://demo.kheops.online/api/studies?album=dTxCc6OJyG -H "Authorization: Bearer oTK6mCWsN8RKRzlWtnK8pd"
```

