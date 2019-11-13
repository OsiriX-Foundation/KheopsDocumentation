---
layout: default
title: OsiriX
parent: Tokens
nav_order: 4
permalink: /docs/tokens/osirix
---

# Osirix

## Configuration

It is possible to connect a Kheops album directly to third-party applications that support DICOMweb. In this example we will show how to connect OsiriX.


* Generate an [Album Token](/docs/tokens/album_tokens). Copy the token for later use.
* In OsiriX, Open File->Preferences.
* Select the *Location* preference. ![OsiriX Preferences](/img/osirix_preferences.png)
* Select *DICOMweb Nodes*.
* Enter the Kheops root URL, `/api` as the path, and give the location a name. ![OsiriX Locations](/img/osirix_enter_location.png)
* Click on the *User* icon to enter the token generated above as the password. Only the password is used by Kheops, the username is ignored. ![OsiriX Password](/img/osirix_enter_password.png)


## Query/Retreive

It is now possible to use OsiriX's standard query, retrieve and store to access the album as a PACS.

![OsiriX Retrieve](/img/osirix_retrieve.png)

![OsiriX Send](/img/osirix_send.png)
