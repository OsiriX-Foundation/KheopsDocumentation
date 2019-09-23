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
