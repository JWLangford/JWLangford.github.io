---
layout: post
title: "Implementing Google Wallet Change Messages"
date: 2025-06-30 12:00:00 +0800
tags: [security, cryptography]
description: "A graphic designer is a professional within the graphic design and graphic arts industry."
status: completed
---

Google Wallet change messages

<!--more-->

## What are change messages

Change messages are a wallet feature enabling messages to be sent to pass holders when values change.

## How to add them to your Google Wallet passes

Google wallet handles change messages by whitelisting object and class fields in each pass type. When one of these values changes, you can tell Google Wallet to send a change message to the affected customer(s).

## Type Specific

Not all pass types have the capability to send change messages. The focus of this piece will be Loyalty Cards.

## How to send

Sending a change message involves setting a particular flag on your UPDATE or PATCH request. For both Object and Class updates, the field is `notifyPreference`.

```
 {
    ...rest,
    notifyPreference = "NOTIFY_ON_UPDATE";
 }
```

## User Flow

Below is an example of what users will see when they receive a push notification for a field

First, a notification badge will appear. You cannot configure this text.

{% include image_caption.html imageurl="/images/google-wallet-change-messages/1.png" title="Apple" caption="This is the caption" %}

If you click on the notification you will be taken into Google Wallet. There will be a message above your pass telling you a change has occurred. You cannot configure this text.

{% include image_caption.html imageurl="/images/google-wallet-change-messages/2.png" title="Apple" caption="This is the caption" %}

Clicking on "Review Update" wil take you to another screen with more context on the change. You cannot configure any of this text.

{% include image_caption.html imageurl="/images/google-wallet-change-messages/4.png" title="Apple" caption="This is the caption" %}
