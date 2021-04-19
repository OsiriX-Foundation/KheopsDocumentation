---
layout: default
title: Logs
parent: Installation
nav_order: 3
permalink: /docs/installation/logs
---

# Logs

This page explains how to KHEOPS manages logs.

## Location and format

Each container write its logs in different paths.

### kheopszipper, kheopsauthorization and kheopsdicomwebproxy

These containers log at the following path : `/usr/local/tomcat/logs` <br>
This directory contain all logs files. 
  - `catalina.{YYYY-MM-DD}.log`
    This file contains the Java logs and stack traces. The log format used is the standard.
  - `localhost_access_log.{YYYY-MM-DD}.txt`
    This file contains all the HTTP access. The log format used is the common log format defined by '%h %l %u %t "%r" %s %b'. [DOC here](http://tomcat.apache.org/tomcat-8.0-doc/config/valve.html#Access_Log_Valve)

### kheopsauthorization
  In addition to common logs, this container logs all actions made by users. These logs are in the `catalina.<YYYY-MM-DD>.log` file and can be find under the level **KHEOPS**. The log contain a set of keys/values that define who do what. The format is the next : \<key\>=\<value\> multiple pair are separated by space.


| key                         | value         | optional | condition / remarques |
|:----------------------------|:--------------|:---------|:----------------------|
| action                      | LIST_ALBUMS, LIST_USERS, NEW_ALBUM, EDIT_ALBUM, ADD_USER, REMOVE_USER, ADD_ADMIN, REMOVE_ADMIN, ALBUM_ADD_FAVORITE, ALBUM_REMOVE_FAVORITE, DELETE_ALBUM, GET_ALBUM, SHARE_STUDY_WITH_USER, SHARE_SERIES_WITH_USER, SHARE_STUDY_WITH_ALBUM, SHARE_SERIES_WITH_ALBUM, REMOVE_SERIES, REMOVE_STUDY, APPROPRIATE_STUDY, APPROPRIATE_SERIES, NEW_CAPABILITY, REVOKE_CAPABILITY, GET_CAPABILITY, GET_CAPABILITIES, INTROSPECT_CAPABILITY, ADD_FAVORITE_SERIES, ADD_FAVORITE_STUDY, REMOVE_FAVORITE_SERIES, REMOVE_FAVORITE_STUDY, QIDO_STUDIES, QIDO_SERIES, QIDO_STUDY_METADATA, POST_COMMENT, LIST_EVENTS, FETCH, TEST_USER, USER_INFO, USERS_LIST, NEW_REPORT_PROVIDER, NEW_REPORT, REPORT_PROVIDER_CONFIGURATION, LIST_REPORT_PROVIDERS, GET_REPORT_PROVIDER, DELETE_REPORT_PROVIDER, EDIT_REPORT_PROVIDER, REPORT_PROVIDER_METADATA, NEW_TOKEN, INTROSPECT_TOKEN, REFRESH_TOKEN_GRANT, AUTHORIZATION_CODE_GRANT, PASSWORD_GRANT, CLIENT_CREDENTIALS_GRANT, JWT_ASSERTION_GRANT, SAML_ASSERTION_GRANT, TOKEN_EXCHANGE_GRANT, NEW_USER, UPDATE_USER, INBOX_INFO, NEW_WEBHOOK, REMOVE_WEBHOOK, GET_WEBHOOK, EDIT_WEBHOOK, LIST_WEBHOOK, TRIGGER_WEBHOOK | false   | - |
| user                        | {user_id}     | false    | - |
| tokenType                   | KEYCLOAK_TOKEN, ALBUM_CAPABILITY_TOKEN, USER_CAPABILITY_TOKEN, PEP_TOKEN, VIEWER_TOKEN, REPORT_PROVIDER_TOKEN | false   | - |
| scope                       | user or album | false    | if 'token_type' is CAPABILITY_TOKEN  |
| clientID                    | {client_id} of the report provider | true    | - |
| albumScope                  | {album_id}    | true     | if 'tokenType' is : ALBUM_CAPABILITY_TOKEN |
| webhookID                   | {webhook_id}  | true     | if 'action' is : NEW_WEBHOOK, REMOVE_WEBHOOK, GET_WEBHOOK, EDIT_WEBHOOK, TRIGGER_WEBHOOK  |
| capabilityID                | {capability_token_id} | true    | if the 'action' is :  |
| sourceIP                    | ip adresse    | true     | - |
| actingParty                 | {admin_id}    | true     | present if the action was made by an admin who impersonnate the user |
| authorizedParty             |  | true       | - |
| authorizedCapabilityTokenId | {capability_token_id} | true    | if the 'action' wase made by a capability token |
| link                        | boolean       | false    | Indicate if the request was a link or not (/api/link/{token}/...) |
| target_user                 | {user_id}     | true     |  if 'action' is : SHARE_STUDY_WITH_USER, SHARE_SERIES_WITH_USER, ADD_USER, REMOVE_USER, ADD_ADMIN, REMOVE_ADMIN |
| album                       | {album_id}    | true     | if action concerne an album |
| fromAlbum                   | {album_id}    | true     | if 'action' is : SHARE_STUDY_WITH_USER, SHARE_SERIES_WITH_USER, SHARE_STUDY_WITH_ALBUM, SHARE_SERIES_WITH_ALBUM from another album |
| events                      | mutations, comments or comments_mutations | true    | if 'action' is : LIST_EVENTS |
| seriesUID                   | {series_uid}  | true     | if 'action' is about a series |
| studyUID                    | {study_uid}   | true     | if 'action' is about a study |
| addSeries                   | boolean       | true     | if 'action' is : NEW_ALBUM or EDIT_ALBUM |
| addUser                     | boolean       | true     | if 'action' is : NEW_ALBUM or EDIT_ALBUM |
| deleteSeries                | boolean       | true     | if 'action' is : NEW_ALBUM or EDIT_ALBUM |
| downloadSeries              | boolean       | true     | if 'action' is : NEW_ALBUM or EDIT_ALBUM |
| sendSeries                  | boolean       | true     | if 'action' is : NEW_ALBUM or EDIT_ALBUM |
| writeComments               | boolean       | true     | if 'action' is : NEW_ALBUM or EDIT_ALBUM |


### kheopsreverseproxy

This container log at the following path : `/var/log/nginx/access.log` and `/var/log/nginx/error.log`

- Accesss logs

> The access logs follow the next pattern : 

> ```
 $remote_addr - $remote_user [$time_local] "$request" $status $body_bytes_sent $upstream_connect_time $upstream_header_time $upstream_response_time $upstream_response_time "$http_referer" "$http_user_agent" "$http_x_forwarded_for"
```

> Example : 
> ```
192.168.144.1 - - [07/Apr/2021:12:55:38 +0000] "GET /api/albums?canAddSeries=true&sort=-last_event_time HTTP/1.1" 200 2 0.000 0.740 0.740 0.740 "http://127.0.0.1/inbox" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.114 Safari/537.36" "-"
```

- Error logs

> Use the level `warning` and the standard format.

### kheopspacsreverseproxy (pep)

This container log at the following path : `/var/log/nginx/access.log` and `/var/log/nginx/error.log`

- Accesss logs

> The access logs follow the next pattern : 

> ```
$remote_addr - $lua_remote_user [$time_local] "$request" $status $body_bytes_sent $upstream_connect_time $upstream_header_time $upstream_response_time $upstream_response_time "$http_referer" "$http_user_agent" "$http_x_forwarded_for" $azp $cap_token $act
```
> with :

> - `lua_remote_user` for the user id or `Fetcher` if the request come from authorization server <br>
> - `azp` for `Authorized party` <br>
> - `cap_token` for the capability token id if the request was made with a capability token (user or album) <br>
> - `act` for `acting party` this field contain the user id who impersonnate the user. <br>

> Example : 
> ```
192.168.96.5 - bh77f5ds-i9n6-vg5d-cfg7-dab2942a561f [07/Apr/2021:14:00:02 +0000] "GET /studies?StudyInstanceUID=1.3.6.1.4.1.5962.1.2.0.1175775772.5732.0&includefield=00081030 HTTP/1.1" 500 183 - - - - "-" "Jersey/2.33 (HttpUrlConnection 11.0.10)" "-" - - -
```

- Error logs

> Use the level `warning` and the standard format.

## Elastic Stack (ELK)

Our Dashboard, Visualisation and Index can be download for adding in your own Kibana [here](https://github.com/OsiriX-Foundation/kibana-initialize/blob/main/export.ndjson). <br>
The Rollup job for the metrics is available [here](https://github.com/OsiriX-Foundation/kibana-initialize/blob/main/rollup_job_kheops_metrics.json)
