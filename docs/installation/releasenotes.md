---
layout: default
title: Release Notes and Upgrade
parent: Installation
nav_order: 1
permalink: /docs/installation/releasenotes
---

# Release Notes and Upgrade Instructions

KHEOPS is composed of a number of Docker Images. All the docker images belonging to a single version and meant to run together are taged with the same version number. As a general rule, upgrading versions is simply a matter up updating the tag on the docker images. **Addition steps** that need to be performed are detailed below. KHEOPS uses [Liquibase](https://www.liquibase.org) to manage the database schema, and the database will be automatically upgraded.

#### As with any ungrade - make sure to perform a backup of the database used by KHEOPS before performing an upgrade.

---

## v0.9.4

**When upgrading, make sure to install and run v0.9.3 prior to installing v0.9.4.**

### Changes
- Removed dependency on Keycloak.
- UI Improvements.

### Upgrade

- Environment variable `KHEOPS_KEYCLOAK_URI` is no longer used.
- Environment variable `KHEOPS_KEYCLOAK_REALMS` is no longer used.
- Environment variable `KHEOPS_KEYCLOAK_CLIENTID` is no longer used.
- Docker secret `kheops_keycloak_clientsecret` is no longer used.
- Docker secret `kheops_metric_ressource_password` is no longer used.
  #### Note when removing secrets - don't forget to remove references to secrets in the *docker-compose.yml*.
- The Keycloak *kheopsAuthorization* Serivce Account is no longer used.

---

## v0.9.3

**Transition version to v0.9.4**

Versions of KHEOPS up to v0.9.2 had a strong dependency on Keycloak. Only the user's ID (sub) was stored in the KHEOPS database, and KHEOPS would use Keycloak APIs and a service account (kheopsAuthorization) to discover a user's name and email address. 

 Starting with v0.9.4, KHEOPS uses exclusively [OpenID Connect (OIDC)](https://openid.net/connect/) for user authentication. v0.9.3 is a transition release that is still dependent on Keycloak, and retrieves the user names and emails from Keycloak at startup.

 While we still expect Keycloak to be used in the majority of cases. It will now be possible to directly use other 

### Changes
- Loads user profile information from Keycloak at startup
- Updates user profile information at each login.
- UI Improvements.

### Upgrade

- Add Environment variable `KHEOPS_OIDC_PROVIDER` which is a URL that points to the OpenIDConnect Provider.

  The full path to the OIDC provider must be set. Its value will typically be `${KHEOPS_KEYCLOAK_URI}/auth/realms/${KHEOPS_KEYCLOAK_REALMS}`
  
  For example: If the `KHEOPS_KEYCLOAK_URI` is `https://keycloak.kheops.online`, and the `KHEOPS_KEYCLOAK_REALMS` is "demo", the value of `KHEOPS_OIDC_PROVIDER` would be `https://keycloak.kheops.online/auth/realms/demo`.

- Environment variables `KHEOPS_ROOT_SCHEME`, `KHEOPS_ROOT_HOST`, and `KHEOPS_ROOT_PORT` are replaced by `KHEOPS_ROOT_URL`.

  The value of `KHEOPS_ROOT_URL` will typically be `${KHEOPS_ROOT_SCHEME}://${KHEOPS_ROOT_HOST}[:${KHEOPS_ROOT_PORT}]`. If the port is the default port for the scheme (e.g., 443 for https), it can be omitted. For example: `https://demo.kheops.online`.

- Environment variable `KHEOPS_UI_KEYCLOAK_CLIENTID` is renamed `KHEOPS_UI_CLIENTID`

- Make sure that the KHEOPS UI login client in Keycloak has the `web-origins`, `email`, and `profile` Client Scope as an Assigned Default Client Scope.

- `KHEOPS_UI_USER_MANAGEMENT_URL` is a new optional environment variable that can be set to provide a page where a user can access user settings. 

  If using Keycloak, this value would probably be set to `${KHEOPS_OIDC_PROVIDER}/account`. For example: `https://keycloak.kheops.online/auth/realms/demo/account`

#### Notes for upgrading for nonsecure install

- Pass the backend docker network hostname of the Keycloak server using the `KHEOPS_OIDC_PROVIDER` environment variable to the *kheops-authorization* Docker container in the *docker-compose.yml* file.

  ``` yaml
      environment:
        KHEOPS_OIDC_PROVIDER:http://keycloak:8080/auth/realms/kheops
  ```

- Pass the frontend URL to the *keycloak* container using the `KEYCLOAK_FRONTEND_URL` environment variable in the *docker-compose.yml* file

  ``` yaml
      environment:
        KEYCLOAK_FRONTEND_URL: http://127.0.0.1:8080/auth
  ```

- Remove the `KHEOPS_KEYCLOAK_URI` environment variable from `kheops-ui` in the *docker-compose.yml* file.

- Use the *osirixfoundation/kheops-authorization:v0.9.3-noissuer* image for the *kheops-authorization* container.


## Older versions

To upgrade from versions older than v0.9.2, please contact us directly at [support@kheops.online](mailto:support@kheops.online?subject=Upgrade%20KHEOPS) or [spalte@naturalimage.ch](mailto:spalte@naturalimage.ch?subject=Upgrade%20KHEOPS).

