---
layout: default
title: Tokens
nav_order: 5
has_children: true
permalink: /docs/tokens
---

# Tokens

KHEOPS Tokens provide a mechanism to give temporary access to KHEOPS resources.

Two classes of tokens exist, *Album* tokens, and *User* tokens. Album tokens give access to a specific KHEOPS album. User tokens give full access to a User's account. Album and User tokens both have a fixed expiration date that must be specified when they are created. Tokens will no longer function after the expiration date has passed. Both token types can be revoked once they are no longer needed or if the token has been compromised.
