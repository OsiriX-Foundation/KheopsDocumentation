---
layout: default
title: User Tokens
parent: Tokens
nav_order: 2
permalink: /docs/tokens/user_tokens
---

# User Tokens

User tokens give full user access to the Kheops platform. They are intended to be used by application that need to perform actions on Kheops in the name of the user. User tokens are OAuth2.0 Access tokens that can be used with Bearer Authorization when calling the [Kheops APIs](https://github.com/OsiriX-Foundation/KheopsAuthorization/wiki).

DICOMweb calls made using a user token will return all studies a user has access to unless an additional [query parameter](/docs/tokens/curl#user-token-parameters) is provided.

In order to create a user token:
* Click on the User Settings Link at the top of the page.
  ![Click User Settings](/img/click_user_settings.png)
* Click on *Token* on the left.
* Click on *+ New token*.
  ![Click New User Token](/img/click_new_user_token.png)

When creating a new token, the following information must be provided:
* Description: A name of the token.
* Scope: Choose *User* for a user token. It is also possible to generate new album tokens from the *User Settings* token panel.
* Expiration date: The date at which the token will expire.

  ![New User Token](/img/new_user_token.png)

The new token is displayed once it has been generated. Make sure to copy and securely store the token. It will not be possible to view the token again later.

![New Token](/img/new_token.png)
