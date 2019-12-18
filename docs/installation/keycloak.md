---
layout: default
title: Keycloak
parent: Installation
nav_order: 1
permalink: /docs/installation/keycloak
---

# Keycloak

KHEOPS delegates user management to Keycloak. With the appropriate configuration, any recent version of Keycloak can be used. There are many different ways to configure Keycloak to work with KHEOP. This document describes the required configuration, along with informational examples for how to acheive this configuration.

## 1. Keycloak KHEOPS Login Client

In order for the KHEOPS UI frontend to be able to intiate a login for a user it will need to use a Keycloak Client. This must be a public OAuth2.0 client with Hybrid flow (Standard flow and Implicit flow enabled).

### Example configuration.
![New Client](/img/keycloak_kheops_login_client.png)


## 2. Client Scope (*kheops*)

KHEOPS will accept JWT Access Tokens issued by Keycloak that contain "*kheops*" within the "*scope*" claim.

### Example for adding the "*kheops*" *scope* claim.

Create a new Client Scope that includes *kheops* in the token scope.
![New Scope Claim](/img/keycloak_kheops_client_scope.png)

Assign the new kheops Client Scope as a default scope for the login client.
![Add Scope Claim](/img/keycloak_kheops_add_scope.png)
.png)

## 3. Service Account

KHEOPS will connect to Keycloak using a service account with a *client_credentials* OAuth2.0 grant in order to retrieve information on users.

### Example

Create a new client with only the Service Account enabled and no other flows enabled.
![New Authorization Client](/img/keycloak_kheops_authorization_client.png)
.png)

The Service Account's credentials (secret) can be found under the Credentials tab.
![New Authorization Client](/img/keycloak_kheops_authorization_credentials.png)
.png)

Add the *view_users* realm-management **client role** under the *Scope* tab of the new client, so that the client is able to *query_groups*, *query_users*, and *view_users*.
![Add Client Roles](/img/keycloak_kheops_authorization_client_roles.png)
.png)

Add the *view_users* realm-management **Service Account** client role to the new client, so that the Service Account is able to *query_groups*, *query_users*, and *view_users*.
![Add Service Account Roles](/img/keycloak_kheops_authorization_service_roles.png)
.png)

## 4. Logging Impersonations

In order for KHEOPS' audit logging to keep track of actions that were exectued by an administrator who has impersonated a user, KHEOPS uses the *"act"* claim definied in the [OAuth 2.0 Token Exchange draft RFC](https://tools.ietf.org/html/draft-ietf-oauth-token-exchange-19#section-4.1) Access Token claim


### Example

Under the login client's *Mappers* tab, click on *Add builtin*. Then, add the *Impersonator User ID* mapper.
![Add Impersonator Mapper](/img/keycloak_kheops_impersonator_builtin.png)

Set the *Token Claim Name* to *act.sub*. It is not neccesary to have the impersonator claim be in the ID token.
![Configure Impersonator Mapper](/img/keycloak_kheops_impersonator_mapper.png)


