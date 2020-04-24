---
layout: default
title: Album Tokens
parent: Tokens
nav_order: 1
permalink: /docs/tokens/album_tokens
---

# Album Tokens

Album Tokens give access to DICOM data within specific album. They are designed to give third-party applications access to the contents of an album. When a third party application uses the token to authenticate with the Kheops DICOMweb endpoint, the album will appear as the content of a DICOMweb PACS to the third-party application. For more technical information see using tokens with [cURL](/docs/tokens/curl)

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

