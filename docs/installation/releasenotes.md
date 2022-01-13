---
layout: default
title: Upgrade and Release Notes
parent: Installation
nav_order: 1
permalink: /docs/installation/releasenotes
---

# Release Notes and Upgrade Instructions

KHEOPS is composed of a number of Docker Images. All the docker images belonging to a single version and meant to run together are tagged with the same version number. As a general rule, upgrading versions is simply a matter of updating the tag on the docker images. **Additional steps** that need to be performed are detailed below. KHEOPS uses [Liquibase](https://www.liquibase.org) to manage the database schema, the database will be automatically upgraded.

#### As with any upgrade - make sure to perform a backup of the database used by KHEOPS before performing an upgrade.

---

## v1.0.7

### Changes

- Docker parent images have been updated.

### Upgrade

- Run `docker-compose pull` then `docker-compose up -d` to update the Elasticsearch and Logstash containers. An earlier version of these was vulnerable to [log4shell](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-44228).
- For the Kheops containers, update the tag to `v1.0.7`.

---
## v1.0.6

### Changes

- Display delete contact email in the UI. When the following environment variables are set, a new line is added to the delete message, prompting the user to contact the email address provided, if he wants to permanently delete the data.

### Upgrade

- New *optional* environment variable `KHEOPS_UI_SHOW_DELETE_CONTACT` for the *kheops-ui* container.
- New *optional* environment variable `KHEOPS_UI_DELETE_CONTACT` for the *kheops-ui* container (obligatory if`KHEOPS_UI_SHOW_DELETE_CONTACT` is set to true).

To use:

- Set the `KHEOPS_UI_SHOW_DELETE_CONTACT` environment variable to true, to enable showing the email address set in the next variable. Use together with the next variable, KHEOPS_UI_DELETE_CONTACT.
- Set the `KHEOPS_UI_DELETE_CONTACT` : if you set this variable to an email address: when a user is deleting a study, he will be notified to contact this email address if he wants to permanently delete it. Only set this when the previous variable KHEOPS_UI_SHOW_DELETE_CONTACT, is set to true.

For example:
```
KHEOPS_UI_SHOW_DELETE_CONTACT=true
KHEOPS_UI_DELETE_CONTACT=[email]@[address].ch
```

---

## v1.0.5

### Changes

- Minor UI updates.
- Display number of instances in a study.
- Fixed an issue with orphan series from Report Providers.

### Upgrade

- No additional upgrade steps are necessary.

---

## v1.0.4

### Changes

- Permanently deleting a series or study is now possible, through an API call, completely removing its data. The permanent deletion is only possible if using the Dcm4Chee PACS - which is by default the case for KHEOPS.
- (API) Mutation: 
  - The [Delete Series API Documentation](https://github.com/OsiriX-Foundation/kheops/wiki/Delete-Series) has been updated to add Permanent Delete, in the Headers section.
  - The [Delete Study API Documentation](https://github.com/OsiriX-Foundation/kheops/wiki/Delete-Study) has been updated to add Permanent Delete, in the Headers section.
  
### Upgrade

(Optional) Only if you want to enable Permanent Delete. Otherwise, no action is necessary.

- Create a new file `kheops_auth_admin_password` in the `secrets` folder eg by running
`printf "%s\n" $(docker run --rm osirixfoundation/openssl rand -base64 32 | tr -dc '[:print:]') > kheops_auth_admin_password`
- Update the `docker-compose.yml` file:
  - In the secrets section of `kheops-authorization` add a new secret: `kheops_auth_admin_password`
  - In the secrets section **at the end of the file** add 
```yaml
  kheops_auth_admin_password:
    file: secrets/kheops_auth_admin_password
```
- Update the `docker-compose.env` file and add `KHEOPS_PROXY_PACS_DCM4CHEE_ARC=http://pacsarc:8080/dcm4chee-arc` after the `KHEOPS_PROXY_PACS_WADO_RS` line.
- Restart KHEOPS by running `docker-compose down` followed by a `docker-compose up -d`

---

## v1.0.2

### Changes

- Minor UI updates.

### Upgrade

- No additional upgrade steps are necessary.

---

## v1.0.1

### Changes

- Better logs when verifying instance (add boolean field 'isValid' and unmatching field in logs).

### Upgrade

- No additional upgrade steps are necessary.

---

## v1.0.0

### Changes

- Validation instance metadata before uploading.

- Fixed issue for metrics.

- Autocomplete for user email address (send, add to an album).

- Fixed silent login.

- UI:
  - Many fixes.
  - Before removing or downgrading an admin from an album, the user will be informed that if he performs this action some capabilities token will be revoked at the same time. Because they were created by the user.
  - Before exiting an album, the user will be informed that if he performs this action some capabilities token will be revoked at the same time. Because they were created by himself.

- Removed filebeat and metricbeat from containers (*kheops-authorization*, *kheops-reverse-proxy* and *pacs-authorization-proxy*). Now one *filebeat* and one *metricbeat* are used as sidecar containers (*kheops-authorization-metricbeat* and *kheops-filebeat-sidecar*).

- (API) Mutation: 
  - Can be filtered by user, seriesUID, studyUID, capabilityTokenID, date, type, ... [(documentation)](https://github.com/OsiriX-Foundation/KheopsAuthorization/wiki/Get-a-list-of-events-(comments-and-mutations))
  - Field `origin` renamed to `source`.
  - Field `capability` renamed to `capability_token`.
  - Add boolean field `is_admin` in `source` if the user is always a member of the album.
  - If the mutaion was made by a report provider or a capability token : the related informations are moved to `source`.
  - `mutation_type=NEW_REPORT` is now `mutation_type=NEW_SERIES` with the `report_provider` in `source`
  - Add field `series` when the mutation type is `ADD_STUDY` and `REMOVE_STUDY`. This new field is an array of series. 
  - Field `series` is now an array with only one series when the mutation type is `ADD_SERIES` and `REMOVE_SERIES`.
  - Add boolean field `is_prensent_in_album` for each series. It indicate if the series is present in the album.

- (API) Comments:
  - Field `origin` renamed to `source`.

- (API) Webhook:
  - Change when a webhook is triggered (for new series). Webhooks are now sent once a timeout is reached follwing the reception of the last instance of a series.
  - New webhooks for *remove_series* and *delete_album*. [(documentation)](https://github.com/OsiriX-Foundation/KheopsAuthorization/wiki#webhooks).
  - If new instances are uploaded to Kheops, a webhook is triggered for each album containing the series.

- (API) Capabilities:
  - Add a field `revoked_by` with the user who revoked the album capability token. [(documentation)](https://github.com/OsiriX-Foundation/kheops/wiki/Capabilities-Tokens-List)

- Database: 
  - New table `event_series`
  - New column `revoked_by` in table `capabilities` set with a user pk when the album capabilitie token is revoked otherwise the field is `Null`.
  - Remove column `series_fk` in table `events`

- Log:
  - kheops-authorizion : the file /usr/local/tomcat/conf/logging.properties rotate after 5 days (90 previously).
  - kheops-reverse-proxy : use logrotate inside the container.

### Upgrade

- Environment variable `KHEOPS_AUTHORIZATION_ENABLE_ELASTIC` is no longer used.
- Environment variable `KHEOPS_AUTHORIZATION_ELASTIC_INSTANCE` is no longer used.
- Environment variable `KHEOPS_AUTHORIZATION_LOGSTASH_URL ` is no longer used.
- Environment variable `KHEOPS_REVERSE_PROXY_ENABLE_ELASTIC` is no longer used.
- Environment variable `KHEOPS_REVERSE_PROXY_ELASTIC_INSTANCE` is no longer used.
- Environment variable `KHEOPS_REVERSE_PROXY_LOGSTASH_URL` is no longer used.
- Environment variable `KHEOPS_PEP_ENABLE_ELASTIC` is no longer used.
- Environment variable `KHEOPS_PEP_ELASTIC_INSTANCE` is no longer used.
- Environment variable `KHEOPS_PEP_LOGSTASH_URL` is no longer used. <br> <br>
- (Recommended) Add a logging driver to all containers to limit the size of generated logs.

```yaml
logging:
  driver: json-file
  options:
    max-file: "10"
    max-size: "10m"
```

Optionally, additional *kheops-authorization-metricbeat* and *kheops-filebeat-sidecar* containers can be added to provide auditing and logging capabilities. If they are present the following environment variables apply.

- New *mandatory* environment variable `KHEOPS_INSTANCES` for the *kheops-authorization-metricbeat* and *kheops-filebeat-sidecar* containers.
- New *mandatory* environment variable `KHEOPS_LOGSTASH_URL` for the *kheops-authorization-metricbeat* and *kheops-filebeat-sidecar* containers.
- The *kheops-authorization-metricbeat* container must be able to connect to the over the network *kheops-authorization* container.
- Add a volume and mount it at /kheops/authorization/logs in  *kheops-filebeat-sidecar* and /usr/local/tomcat/logs in *kheops-authorization*.
- Add a volume and mount it at /kheops/reverseproxy/logs in  *kheops-filebeat-sidecar* and /var/log/nginx in *kheops-reverse-proxy*.
- Add a volume and mount it at /kheops/pep/logs in  *kheops-filebeat-sidecar* and /var/log/nginx in *pacs-authorization-proxy*.
- Add a volume and mount it at /registry in  *kheops-filebeat-sidecar* is recommended for the filebeat rsgistry.

---

## v0.9.5

### Changes

- Many UI fixes.

- Fixed issue where the UI might not load after a long idle time.

### Upgrade

- No additional upgrade steps are necessary.

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
- The Keycloak *kheopsAuthorization* Service Account is no longer used.

---

## v0.9.3

**Transition version to v0.9.4**

Versions of KHEOPS up to v0.9.2 had a strong dependency on Keycloak. Only the user's ID (sub) was stored in the KHEOPS database, and KHEOPS would use Keycloak APIs and a service account (kheopsAuthorization) to discover a user's name and email address. 

 Starting with v0.9.4, KHEOPS uses exclusively [OpenID Connect (OIDC)](https://openid.net/connect/) for user authentication. v0.9.3 is a transition release that is still dependent on Keycloak, and retrieves the user names and emails from Keycloak at startup.

 While we still expect Keycloak to be used in the majority of cases. It will now be possible to use other OIDC providers directly, without installing Keycloak.

### Changes
- Loads user profile information from Keycloak at startup.
- Updates user profile information at each login.
- UI Improvements.

### Upgrade

- Add Environment variable `KHEOPS_OIDC_PROVIDER` which is a URL that points to the OpenIDConnect Provider.

  The full path to the OIDC provider must be set. Its value will typically be `${KHEOPS_KEYCLOAK_URI}/auth/realms/${KHEOPS_KEYCLOAK_REALMS}`
  
  For example: If the `KHEOPS_KEYCLOAK_URI` is `https://keycloak.kheops.online`, and the `KHEOPS_KEYCLOAK_REALMS` is `demo`, the value of `KHEOPS_OIDC_PROVIDER` would be `https://keycloak.kheops.online/auth/realms/demo`.

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

- The `-noissuer` tag for the *osirixfoundation/kheops-authorization* image is no longer needed.


## Older versions

To upgrade from versions older than v0.9.2, please contact us directly at [support@kheops.online](mailto:support@kheops.online?subject=Upgrade%20KHEOPS) or [spalte@naturalimage.ch](mailto:spalte@naturalimage.ch?subject=Upgrade%20KHEOPS).

