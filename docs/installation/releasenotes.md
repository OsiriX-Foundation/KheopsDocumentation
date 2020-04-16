---
layout: default
title: Release Notes and Upgrade
parent: Installation
nav_order: 1
permalink: /docs/installation/releasenotes
---

# Release Notes and Upgrade Instructions

## v0.9.4
---

**When upgrading, make sure to install and run v0.9.3 prior to installing v0.9.4.**

### Changes
- removed dependency on Keycloak.
- UI Improvements.

### Upgrade

- Environment variable `KHEOPS_KEYCLOAK_URI` is no longer used.
- Environment variable `KHEOPS_KEYCLOAK_REALMS` is no longer used.
- Environment variable `KHEOPS_KEYCLOAK_CLIENTID` is no longer used.
- Docker secret `kheops_keycloak_clientsecret` is no longer used.
- The Keycloak *kheopsAuthorization* Serivce Account is no longer used.

## v0.9.3
---

**Transition version to v0.9.4**

Versions of KHEOPS up to v0.9.2 had a strong dependecy on Keycloak. Only the user's ID (sub) was stored in the KHEOPS database, and KHEOPS would use Keycloak APIs and a service account (kheopsAuthorization) to discover a user's name and email address. 

 Starting with v0.9.4, KHEOPS uses exclusivly OpenID Connect (OIDC) for user authentication. v0.9.3 is a transtion release that is still dependent on Keycloak, and retrieves the user names and emails from Keycloak at startup.

### Changes
- Loads user profile information from Keycloak at startup
- Updates user profile information at each login.
- UI Improvements.

### Upgrade

- Add Environment variable `KHEOPS_OIDC_PROVIDER` which is a URL that points to the OpenIDConnect Provider.

  The full path to the OIDC provider must be set. Its value will typically be `${KHEOPS_KEYCLOAK_URI}/auth/realms/${KHEOPS_KEYCLOAK_REALMS}`
  
  For example: If the `KHEOPS_KEYCLOAK_URI` is "https://keycloak.kheops.online", and the `KHEOPS_KEYCLOAK_REALMS` is "demo", the value of `KHEOPS_OIDC_PROVIDER` would be "https://keycloak.kheops.online/auth/realms/demo".

- Environment variables `KHEOPS_ROOT_SCHEME`, `KHEOPS_ROOT_HOST`, and `KHEOPS_ROOT_PORT` are replaced by `KHEOPS_ROOT_URL`.

  The value of `KHEOPS_ROOT_URL` will typically be `${KHEOPS_ROOT_SCHEME}://${KHEOPS_ROOT_HOST}[:${KHEOPS_ROOT_PORT}]`. If the port is the default port for the scheme (e.g., 443 for https), it can be omitted.

- Environment variable `KHEOPS_UI_KEYCLOAK_CLIENTID` is renamed `KHEOPS_UI_CLIENTID`

- Make sure that the KHEOPS UI login client in Keycloak  has the `web-origins` Client Scope as an Assigned Default Client Scope.

## Older versions

To upgrade from versions older than v0.9.2, please contact us directly at [support@kheops.online](mailto:support@kheops.online?subject=Upgrade%20KHEOPS) or [spalte@naturalimage.ch](mailto:spalte@naturalimage.ch?subject=Upgrade%20KHEOPS).
