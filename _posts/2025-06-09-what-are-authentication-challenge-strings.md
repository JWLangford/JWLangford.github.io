---
layout: post
title: "What Are Authentication Challenge Strings"
date: 2025-06-09 12:00:00 +0800
tags: []
description: "A graphic designer is a professional within the graphic design and graphic arts industry."
status: in-progress
---

- What are challenge strings
  Challenge strings are part of challenge-response authentication, where users prove knowledge of a secret without transmitting it. Challenge-response authentication is a step up in security from standard logins requiring users to transmit secrets (passwords) to a server for authentication.

<!--more-->

Asymetric Authentication
Asymentric authentication involves a public and private key pair. The client generates the keys, keeps the private key and sends the public to the server to be stored against the user.

When the client makes any subsequent requests, the server will create and store a random challenge string. The string is sent back to the client where it uses it's stored private key to sign the challenge string.

When the server receives the signed challenge string, it uses the stored public key to validate the signature.

Symetric Authentication
Symetric authentication requires the client and the server to have a shared secret. The client generates the secret, stores it locally and sends a copy to the server.

When the client makes any subsequent requests, the server will create and store a random challenge string. The string is sent back to the client where it uses it's stored secret to compute an HMAC of the challenge string and sends it back to the server.

When the server receives the hashed challenge string, it looks up the stored original challenge and the shared secret for the user, computes its own HMAC of the challenge string and compares it to the one provided by the client.

Security

Transmission
One advantage asymetric authentication has over symetric authentication is symetric authentication requires the transmission of sensitive information at least once. You also need to make sure the client and the server use the same hashing algorythum.

Asymetric authentication never needs to transmit its private key, it always stays with the client.

Replay attacks
In either scenario, it's a good practise to store the generated challenge strings. When you create a new challenge string, ensure each challenge string is unique and hasn’t been reused.

Challenge Strength
When creating challenge strings, make sure the strings are long and cryptographically generated.

Challenge Window
Its a good practise to limit the window of opportunity for a client to respond to a given challenge.

- Where are they used
  A common use case for this kind of challenge authentication is biometric login for mobile apps. For example, when a user enables faceID, their device can generate a public and private key pair. The private key can be stored on the phone and the public key can be sent to the server. Any subsequent biometric authentications would go through asymetric authentication, meaning the user never needs to give a password to re authenticate themselves.
