---
layout: default
title: Installation
nav_order: 7
has_children: true
permalink: /docs/installation
---

# Getting started with a local installation

At its core KHEOPS is an authorization layer placed in front of a DICOMweb capable PACS. KHEOPS is composed of multiple docker containers typically orchestrated  using Docker-Compose or Kubernetes.

We are always happy to respond to any questions. Feel free to contact us at [contact@kheops.online](mailto:contact@kheops.online).

## A simple insecure local server

Below are instruction for getting a basic instance of KHEOPS up and running. This configuration does not provide security. In order to be properly secured,  KHEOPS and Keycloak must be placed behind TLS (https) connections and the Keycloak configuration must be updated. Nevertheless, this install will provide a starting point that can be modified as needed for the local environment.

1. Install Docker (latest version)
2. Install Docker-Compose (latest version)
3. Make sure that the current user is in the *docker* group.
4. Run the following command:

```shell
bash <(curl -sL https://raw.githubusercontent.com/OsiriX-Foundation/KheopsOrchestration/insecure-install-v1.0.0/kheopsinstall.sh)
```

This script will create a new directory named `kheops` in which it will download docker-compose configuration files, a keycloak realm configuration, and generate the necessary secrets.

Once installed, Keycloak will be available at [http://127.0.0.1:8080](http://127.0.0.1:8080), 
Kibana will be available at [http://127.0.0.1:8081](http://127.0.0.1:8081) and
KHEOPS will be available at [http://127.0.0.1](http://127.0.0.1). When you first connect to KHEOPS
you will be redirected to the Keycloak login screen. The `Register` link will be available to
create a new KHEOPS account. 

**notes**
- On MacOS, the default Docker memory limit of 2GB is not sufficient. 4GB is safer. 
- Also on MacOS, Safari does not trust connections to 127.0.0.1, so when loading a study in the OHIF viewer (which is loaded with https), Safari refuses the mixed security connection. Chrome respects the standards and considers connections to 127.0.0.1 to be secure so this issue does not occur.

## Making it secure

In a production environment, Keycloak should be set up on a separate domain. KHEOPS interacts with Keycloak using the Authorization Code flow. Please refer to the Keycloak documentation for instruction on how to properly secure Keycloak. KHEOPS specific Keycloak configuration steps are described [here](installation/keycloak). Keycloak must be configured to work using a secured TLS (https) connection in order to secure user credentials.

### Using Let's Encrypt

A Let's Encrypt enabled reverse proxy for KHEOPS is available. To use it, replace the `-insecure` portion of the tag on the the `kheops-reverse-proxy` with `-letsencrypt`. When using the Let's Encrypt enabled version of the reverse proxy, the `KHEOPS_ROOT_URL` environment variable must be a URL accessible from the general internet, and the `LETS_ENCRYPT_EMAIL` environment variable must be set to the email with which the domain will be registered. Add a volume that will store the Let's Encrypt certificates in order to avoid having Let's Encrypt re-issue certificates every time the container is run.

1. In the `docker-compose.env` file change the following:
    - Change the `KHEOPS_ROOT_URL` to your domain.
    - Change the `KHEOPS_OIDC_PROVIDER` to your secured Keycloak or other OIDC Provider.
    - If needed, change the `KHEOPS_UI_CLIENTID`.
2. In the ``kheops-authorization`` section of the `docker-compose.yml` file change the following:
    - Remove the `KHEOPS_OIDC_PROVIDER` environment variable.
3. In the ``kheops-reverse-proxy`` section of the `docker-compose.yml` file change the following:
    - Replace the `-insecure` portion of the tag with `-letsencrypt`.
    - Add an extra_hosts section that loopbacks the root.

        ```yaml
        extra_hosts:
          - "your.domain.here:127.0.0.1"
        ```

    - Modify the ports section as follows.

        ```yaml
        ports:
          - "80:80"
          - "443:443"
        ```
        
    - Modify the volumes section of the container as follows.
        ```yaml
        volumes:
          - letsencrypt:/etc/letsencrypt
        ```

    - Add the letsencrypt volume to the list of volumes to create at the bottom of the file.
         ```yaml
         volumes:
           letsencrypt:
         ```

### Using a custom certificate

It is possible to use a custom TLS certificate. To use it, replace the `-insecure` portion of the tag on the the `kheops-reverse-proxy`.

1. Place the certificate private key file in `secrets/privkey1.pem`.
2. Place the certificate chain file in `secrets/fullchain1.pem`.
3. In the `docker-compose.env` file change the following:
    - Change the `KHEOPS_ROOT_URL` to your domain.
    - Change the `KHEOPS_OIDC_PROVIDER` to your secured Keycloak or other OIDC Provider.
    - If needed, change the `KHEOPS_UI_CLIENTID`.
4. In the ``kheops-authorization`` section of the `docker-compose.yml` file change the following:
    - Remove the `KHEOPS_OIDC_PROVIDER` environment variable.
5. In the ``kheops-reverse-proxy`` section of the `docker-compose.yml` file change the following:
    - Remove the `-insecure` portion of the tag.
    - Add an extra_hosts section that loopbacks the root.

        ```yaml
        extra_hosts:
          - "your.domain.here:127.0.0.1"
        ```

    - Modify the ports section as follows.

        ```yaml
        ports:
          - "80:80"
          - "443:443"
        ```

    - Add a secrets section.

        ```yaml
        secrets:
          - privkey1.pem
          - fullchain1.pem
        ```

6. In secrets section, add the following secrets:

    ```yaml
    privkey1.pem:
      file: secrets/privkey1.pem
    fullchain1.pem:
      file: secrets/fullchain1.pem
    ```

---

### Removing Kibana and logs management

**In the *docker-compose.yml* file :**
- Remove services : *kibana, elasticsearch, logstash, kheops-authorization-metricbeat and kheops-filebeat-sidecar*
- Remove networks : *beats_network, elk_network* in all the *docker-compose.yml*
- Remove volumes : *elastic_data, logs_pep, logs_reverse_proxy, logs_auth* in all the *docker-compose.yml*

**In the *docker-compose.env* file :** 
- Remove *KHEOPS_INSTANCES* and *KHEOPS_LOGSTASH_URL*

### Sending logs to an ELK in production

**In the *docker-compose.yml* file :**
- Remove services : *kibana, elasticsearch and logstash*
- Remove network : *beats_network* in all the *docker-compose.yml*
- Remove volume : *elastic_data* in all the *docker-compose.yml*

**In the *docker-compose.env* file : **
- Edit *KHEOPS_LOGSTASH_URL* with your own logstash url

**In your ELK**

- Import logstash config [here](https://github.com/OsiriX-Foundation/logstash/blob/main/logstash.conf)
- Import index pattern, visualisation and dashboard in your Kibana from [here](https://github.com/OsiriX-Foundation/kibana-initialize/blob/main/export.ndjson)
- Configure the rollup job `rollup_job_kheops_metrics` by doing a *PUT* _rollup/job/rollup_job_kheops_metrics with the following json [here](https://github.com/OsiriX-Foundation/kibana-initialize/blob/main/rollup_job_kheops_metrics.json)
- Start de rollup job : *POST* _rollup/job/rollup_job_kheops_metrics/_start

# Dependencies on External Services

A KHEOPS instance consists of the KHEOPS Docker containers interacting with the following services.

- ## A DICOMweb Capable PACS

  KHEOPS uses strict DICOMweb APIs to communicate with the backing PACS server, making it possible to use any DICOMweb capable PACS. In practice KHEOPS has been tested with DCM4CHEE and Google Cloud Platform Healthcare API.

  ### DCM4CHEE

  KHEOPS has received the most testing using [DCM4CHEE](https://github.com/dcm4che/dcm4chee-arc-light/wiki) as the backing PACS. Since it is the role of KHEOPS to handle authorization and secure access, DCM4CHEE is run in an [unsecured configuration](https://github.com/dcm4che/dcm4chee-arc-light/wiki/Run-minimum-set-of-archive-services-on-a-single-host), and is made available only to KHEOPS. 

  When using DCM4CHEE, the required minimum version is 19.1.

  ### Google Cloud Platform

  KHEOPS can also run on Google Cloud Platform using the [Cloud Healthcare API](https://cloud.google.com/healthcare) using [DICOM stores](https://cloud.google.com/healthcare/docs/how-tos/dicom). Unfortunately, DICOM stores within the Cloud Healthcare API [only support a subset](https://cloud.google.com/healthcare/docs/dicom) of DICOMweb. Specific Docker images that work around these limitations are available.

  When running on Google Cloud Healthcare, KHEOPS will access the DICOMweb Store using a [Service Account](https://cloud.google.com/iam/docs/service-accounts).
  
- ## An OpenID Connect Capable Authorization Server

  For user authentication, KHEOPS behaves as an [OpenID Connect (OIDC)](https://openid.net/connect/) Client. 

  ### Keycloak

  KHEOPS has received the most testing using [Keycloak](https://www.keycloak.org) as the OIDC Provider. Instructions for configuring Keycloak are available [here](installation/keycloak).

- ## PostgreSQL Database

  KHEOPS stores all state to a PostgreSQL database. Versions 9.6, 10, and 12 have been used in the past. Other versions will likely also work.

---
# Architecture Diagram

| ![Architecture](/img/architecture.svg) | 
|:--:| 
| *Architecture of a KHEOPS installation* |

