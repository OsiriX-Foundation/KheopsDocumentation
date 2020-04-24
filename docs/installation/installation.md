---
layout: default
title: Installation
nav_order: 7
has_children: true
permalink: /docs/installation
---

# Getting started with  local installation

At its core KHEOPS is an authorization layer placed in front of a "DICOMweb" capable PACS. KHEOPS is composed of multiple docker containers typically orchestrated together using Docker-Compose or Kubernetes.

---

# Dependencies on External Services

A KHEOPS instance consists of the KHEOPS Docker containers interacting with the following services.

- ## A DICOMweb Capable PACS

  KHEOPS uses strict DICOMweb APIs to communicate with the backing PACS server, making it possible to use any DICOMweb capable PACS. In practice KHEOPS has been tested with DCM4CEE and Google Cloud Platform Healthcare API.

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

