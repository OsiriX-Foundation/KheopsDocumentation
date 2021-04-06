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

- kheopszipper, kheopsauthorization and kheopsdicomwebproxy

These containers log at the following path : /usr/local/tomcat/logs
This directory contain all logs files. 
  - cataline.<YYYY-MM-DD>.log
    This file contains the Java logs and stack traces. The log format used is the standard *****TODO: add link or more****
  - localhost_access_log.<YYYY-MM-DD>.txt
    This file contains all the HTTP access. The log format used is the common log format defined by '%h %l %u %t "%r" %s %b'. [DOC here](http://tomcat.apache.org/tomcat-8.0-doc/config/valve.html#Access_Log_Valve)

- kheopsauthorization
  In addition to common logs, this container logs all actions made by users. These logs are in the cataline.<YYYY-MM-DD>.log file and can be find under the level ´KHEOPS´. The log contain a set of keys/values that define who do what. The format is the next : <key>=<value> multiple pair are separated by space.


| key                         | value | optional | condition | remarques |
|:----------------------------|:------|:--------|:----------|:----------|
| action                      | LIST_ALBUMS, LIST_USERS, NEW_ALBUM, EDIT_ALBUM, ADD_USER, REMOVE_USER, ADD_ADMIN, REMOVE_ADMIN, ALBUM_ADD_FAVORITE, ALBUM_REMOVE_FAVORITE, DELETE_ALBUM, GET_ALBUM, SHARE_STUDY_WITH_USER, SHARE_SERIES_WITH_USER, SHARE_STUDY_WITH_ALBUM, SHARE_SERIES_WITH_ALBUM, REMOVE_SERIES, REMOVE_STUDY, APPROPRIATE_STUDY, APPROPRIATE_SERIES, NEW_CAPABILITY, REVOKE_CAPABILITY, GET_CAPABILITY, GET_CAPABILITIES, INTROSPECT_CAPABILITY, ADD_FAVORITE_SERIES, ADD_FAVORITE_STUDY, REMOVE_FAVORITE_SERIES, REMOVE_FAVORITE_STUDY, QIDO_STUDIES, QIDO_SERIES, QIDO_STUDY_METADATA, POST_COMMENT, LIST_EVENTS, FETCH, TEST_USER, USER_INFO, USERS_LIST, NEW_REPORT_PROVIDER, NEW_REPORT, REPORT_PROVIDER_CONFIGURATION, LIST_REPORT_PROVIDERS, GET_REPORT_PROVIDER, DELETE_REPORT_PROVIDER, EDIT_REPORT_PROVIDER, REPORT_PROVIDER_METADATA, NEW_TOKEN, INTROSPECT_TOKEN, REFRESH_TOKEN_GRANT, AUTHORIZATION_CODE_GRANT, PASSWORD_GRANT, CLIENT_CREDENTIALS_GRANT, JWT_ASSERTION_GRANT, SAML_ASSERTION_GRANT, TOKEN_EXCHANGE_GRANT, NEW_USER, UPDATE_USER, INBOX_INFO, NEW_WEBHOOK, REMOVE_WEBHOOK, GET_WEBHOOK, EDIT_WEBHOOK, LIST_WEBHOOK, TRIGGER_WEBHOOK | false   | - |   |
| user                        | <user_id> | false   | - |
| tokenType                   | KEYCLOAK_TOKEN, ALBUM_CAPABILITY_TOKEN, USER_CAPABILITY_TOKEN, PEP_TOKEN, VIEWER_TOKEN, REPORT_PROVIDER_TOKEN | false   | - |   |
| scope                       |  | false   | - |   |
| clientID                    |  | true    | - |   |
| albumScope                  |  | true    | - |   |
| webhookID                   | <webhook_id> | true    | - | if action is : NEW_WEBHOOK, REMOVE_WEBHOOK, GET_WEBHOOK, EDIT_WEBHOOK, TRIGGER_WEBHOOK  |
| capabilityID                | <capability_token_id> | true    | - |   |
| sourceIP                    | ip adresse | true    | - |   |
| actingParty                 |  | true    | - |   |
| authorizedParty             |  | true    | - |   |
| authorizedCapabilityTokenId |  | true    | - |   |
| link                        | boolean | false   | - |   |
| target_user                 | <user_id> | true    | - |  if 'action' is : SHARE_STUDY_WITH_USER, SHARE_SERIES_WITH_USER, ADD_USER, REMOVE_USER, ADD_ADMIN, REMOVE_ADMIN |
| album                       | <album_id> | true    | - | if action concerne an album |
| fromAlbum                   | <album_id> | true    | if 'action' is : SHARE_STUDY_WITH_USER, SHARE_SERIES_WITH_USER, SHARE_STUDY_WITH_ALBUM, SHARE_SERIES_WITH_ALBUM from another album |   |
| events                      |  | true    | - |   |



  TODO lister chaque key et les valeurs (type explication, lien etc) 


- kheopsreverseproxy



- kheopspacsreverseproxy (pep)




## Elastic Stack (ELK)


