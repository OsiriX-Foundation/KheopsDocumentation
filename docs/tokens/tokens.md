---
layout: default
title: Tokens
nav_order: 2
has_children: true
permalink: /docs/tokens
---

# Tokens

Kheops Tokens provide a mechanism to give temporary access to Kheops resources.

Two classes of tokens exist, *Album* tokens, and *User* tokens. Album tokens give access to a specific Kheops album. User tokens give full access to a User's account. When both Album, and User tokens are generated an expiration date must be specified. The token will no longer function once the expiration date passes. Both token types can also be revoked once they are no longer needed or if the token has been compromised.

## Album Tokens

Album Tokens give access to a specific album. They are designed to be used give a third-party application access to an album. When a third party application uses the token to authenticate with the Kheops DICOMweb endpoint, the album will appear as the content of a DICOMweb PACS to the thrid-party application. For more technical information see using tokens with [cURL](/docs/tokens/curl)

In order to create a new token:
* Open an album.
* Click on the *Settings* button on the top right.
* Click on the *Token* button on the left.
* Click on *+ New Token*.

![New Token Button](/img/click_new_album_token.png)

When creating a new token, the following information must be provided:
* Description: A name of the token.
* Permissions:
  - Write: Can the token be used to add DICOM instances to the album.
  - Read: Can the token be used to read DICOM data from the album.
  - Show Download Button: If the token is used in a graphical interface, should the download button be displayed.
  - Add to Album / Inbox: Can series from the album be sent to other albums or to token bearer's inbox.
* Expiration date: The date at which the token will expire.

Once the above information has been filled out, click on *Create* to create the new token.

![New Album Token](/img/new_album_token.png)

The new token is displayed once it has been generated. Make sure to copy and securely store the token. It will not be possible to view the token again later.

![New Token](/img/new_token.png)

## User Tokens

User tokens give full user access to the Kheops platform. They are intended to be used by application that need to perform actions on Kheops in the name of the user. User tokens are OAuth2.0 Access tokens that can be used with Bearer Authorization when calling the [Kheops APIs](https://github.com/OsiriX-Foundation/KheopsAuthorization/wiki).

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
