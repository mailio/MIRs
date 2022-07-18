---
mir: 9
title: Message API
description: Message management API
author: Igor Rendulic (@igorrendulic)
status: Draft
type: API
category: API
created: 2022-07-07
---

## Abstract

An interface between a client and server, message API enables clients to retrieve received messages from server and send messages.

## Motivation

Basic operations of any communication service.

## Specification

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

### Get single message

**RECOMMENDED** URL path for REST API: `GET https://{path}/v1/message/{messageId}`

Example result:

```json
{
  "id": "9m4e2mr0ui3e8a215n4g",
  "senderPublicKey": "wNHMVGt2B0QpUwMpTMTOlcgMC1k8UdKjYpgGjRElfVc=",
  "receiverPublicKey": "fh8zNMF0QXL3FXtpqsl34fMkG9ly8JgzU+Efm9mkMy4=",
  "messageId": "20220708104022.28235644.30655305@mail.io",
  "ownerAddressHex": "0xd6df3963a4cf40df960f69f21f63d4e95c368882",
  "read": true,
  "folder": "trusted",
  "contentType": "enc/html/text",
  "contentRef": "MS2EZMmnbvXiJemW8cyzU7l5bsPco+joRDPi7MoIFjr9M0A/7VWx7WXiQIRZBxyTUfUgXdmi6XwnRXP70mswN0M4",
  "contentSource": "ipfs",
  "boxSharedKey": "0mwmFiF96ZMeX29sKQ7KmZUZe5h6LhHSII/AfQsqfkMA==",
  "attachmentRefs": [
    "nmstKExzlxkm78JKaNB+7y10IlnnEnhZSN7XwN29a7eRaobwtzKfK5uxYg8TbBmCT6uSu3YghifL+2gKNfgCGTDu",
    "Fqyxr/gOL+aeRZVJnp4ax3EPA/chv79A2P+wWCxheYIigdD+tOCmciUj11hQcld7QAjteMTCADkclHvHr2MyU8PW"
  ],
  "created": 1657291984,
  "modified": 1657291984
}
```

### List messages

**RECOMMENDED** URL path for REST API: `GET https://{path}/v1/message`

**RECOMMENDED** URL path for REST API for paging: `GET https://{path}/v1/message?from=1657291983000&limit=100`

**RECOMMENDED** URL path for REST API for paging with limit: `GET https://{path}/v1/message?from=1657291983000&to=1657291985000&limit=20`

Example result (**MAY** have encrypted messages and **MAY** have unencrypted messages at the same time in the list):

```json
[
  {
    "id": "9m4e2mr0ui3e8a215n4g",
    "senderPublicKey": "wNHMVGt2B0QpUwMpTMTOlcgMC1k8UdKjYpgGjRElfVc=",
    "receiverPublicKey": "fh8zNMF0QXL3FXtpqsl34fMkG9ly8JgzU+Efm9mkMy4=",
    "messageId": "20220708104022.28235644.30655305@mail.io",
    "ownerAddressHex": "0xd6df3963a4cf40df960f69f21f63d4e95c368882",
    "read": true,
    "folder": "trusted",
    "contentType": "enc/html/text",
    "contentRef": "MS2EZMmnbvXiJemW8cyzU7l5bsPco+joRDPi7MoIFjr9M0A/7VWx7WXiQIRZBxyTUfUgXdmi6XwnRXP70mswN0M4",
    "contentSource": "ipfs",
    "boxSharedKey": "0mwmFiF96ZMeX29sKQ7KmZUZe5h6LhHSII/AfQsqfkMA==",
    "attachmentRefs": [
      "nmstKExzlxkm78JKaNB+7y10IlnnEnhZSN7XwN29a7eRaobwtzKfK5uxYg8TbBmCT6uSu3YghifL+2gKNfgCGTDu",
      "Fqyxr/gOL+aeRZVJnp4ax3EPA/chv79A2P+wWCxheYIigdD+tOCmciUj11hQcld7QAjteMTCADkclHvHr2MyU8PW"
    ],
    "created": 1657291984000,
    "modified": 1657291984000
  },
  {
    "id": "9m4e2mr0ui3e8a215n47",
    "receiverPublicKey": "fh8zNMF0QXL3FXtpqsl34fMkG9ly8JgzU+Efm9mkMy4=",
    "messageId": "62c82df0b6fae_5febd77828017a@mail.io",
    "ownerAddressHex": "0xd6df3963a4cf40df960f69f21f63d4e95c368882",
    "read": false,
    "folder": "other",
    "contentType": "mime",
    "contentRef": "......SGVsbG8sIOS4lueVjA==....", // base64 email mime json
    "contentSource": "s3",
    "created": 1657292590000,
    "modified": 1657292590000
  }
]
```

### Parse Mime

A helper method for clients to utilize servers capability of parsing standard Mime messages.

**RECOMMENDED** URL path for REST API: `POST https://{path}/v1/parsemime`

Example request payload:

```json
{
  "mime": "CglEZWxpdmVyZWQtVG86IGlnb3IuYW1wbGlvQGdtYWlsLmNvbQpSZWNlaXZlZDogYnkgMjAwMjphMTc6OTBhOjM3MDk6MDowOjA6..." // base64 encoded mime message
}
```

Example response:

```json
{
  "contentType": "multipart/alternative",
  "headers": [
    {
      "name": "Mime-Version",
      "value": "1.0"
    }
  ],
  "subject": "hello from me",
  "sender": "Igor <test@mail.io>",
  "messageId": "<0101016e492c433b-d860c26f-c22a-4523-9656-cbb2f964a98d-000000@us-west-2.amazonses.com>",
  "dateAsString": "Fri, 8 Jul 2022 04:01:07 +0000",
  "to": ["test@mail.io"],
  "allRecipients": ["test@mail.io"],
  "body": {
    ...
  },
  "date": "Fri, 8 Jul 2022 04:01:07 +0000"
}
```

### Send encrypted message

Messages **MAY** be sent as `encrypted Mailio` messages or `SMTP Mime` messages (unencrypted)

**RECOMMENDED** URL path for REST API: `POST https://{path}/v1/message`

```json
[
  {
    "senderPublicKey": "wNHMVGt2B0QpUwMpTMTOlcgMC1k8UdKjYpgGjRElfVc=",
    "receiverPublicKey": "fh8zNMF0QXL3FXtpqsl34fMkG9ly8JgzU+Efm9mkMy4=",
    "contentType": "enc/html/text",
    "contentRef": "MS2EZMmnbvXiJemW8cyzU7l5bsPco+joRDPi7MoIFjr9M0A/7VWx7WXiQIRZBxyTUfUgXdmi6XwnRXP70mswN0M4",
    "contentSource": "ipfs",
    "boxSharedKey": "0mwmFiF96ZMeX29sKQ7KmZUZe5h6LhHSII/AfQsqfkMA==",
    "attachmentRefs": [
      "nmstKExzlxkm78JKaNB+7y10IlnnEnhZSN7XwN29a7eRaobwtzKfK5uxYg8TbBmCT6uSu3YghifL+2gKNfgCGTDu",
      "Fqyxr/gOL+aeRZVJnp4ax3EPA/chv79A2P+wWCxheYIigdD+tOCmciUj11hQcld7QAjteMTCADkclHvHr2MyU8PW"
    ],
    "emailPinDuration": "7d" // time duration formats can be e.g.: 7d, 1m, 72h
  },
  {
    "senderPublicKey": "wNHMVGt2B0QpUwMpTMTOlcgMC1k8UdKjYpgGjRElfVc=",
    "receiverPublicKey": "...",
    "contentType": "enc/html/text",
    "contentRef": "...",
    "contentSource": "ipfs",
    "boxSharedKey": "...",
    "attachmentRefs": ["...", "..."],
    "emailPinDuration": "7d" // time duration formats can be e.g.: 7d, 1m, 72h
  }
]
```

### Send unencrypted message

**RECOMMENDED** URL path for REST API: `POST https://{path}/v1/mimemessage`

```json
{
  "subject": "My subject",
  "to": ["test@mail.io", "someone@mail.io"],
  "cc": ["cc@mail.io"],
  "bcc": ["bcc@mail.io"],
  "body": "<div>html content here</div>",
  "attachments": [
    {
      "contentSource": "s3",
      "filename": "myfile1.png",
      "contentRef": "bucket/myfile"
    },
    {
      "contentSource": "ipfs",
      "filename": "myfile2.png",
      "contentRef": "QmbWqxBEKC3P8tqsKc98xmWNzrzDtRLMiMPL8wBuTGsMnR"
    }
  ]
}
```

Attachment content must be copied and added to Email Mime Message.

## Rationale

The API embraces both unencrypted messages and encrypted messages:

- Retrieve message that may be encrypted or unencrypted, thus supporting SMTP alongside with Mailio protocol
- Parse Email Mime message, a helper method for parsing messages for the clients
- A message sending for unencrypted messages - SMTP and a message sending for enrypted messages

This design is motivated by requirements to seamlessly support SMTP send protocol as well as Mailio protocol where Mailio protocol could eventually be implemented as part of SMTP protocol.

## Backwards Compatibility

All APIs have a version degined in the path iteself as in example above: `v1`.

## Security Considerations

All API communication must be conducted over TLS 1.2 or newer protocol

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
