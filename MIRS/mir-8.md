---
mir: 8
title: Folders API
description: Email Folders API operations
author: Igor Rendulic (@igorrendulic)
status: Draft
type: API
category: API
created: 2022-07-07
---

## Abstract

Folders API is able to communicate to manage masage folders on the server side:

- list all folders with read/unread messages
- delete a folder
- create new folder

## Motivation

Enables users to organize their messages.

## Specification

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

Folder **MUST** have an `id` and `name`.

TotalMessages and Unread Messages are **RECOMMENDED**.

```go
type Folder struct {
	ID             string `json:"id"`                       // id of the folder
	Name           string `json:"name" validate:"required"` // name is required
	TotalMessages  int64  `json:"totalMessages"`            // total number of messages in the folder
	UnreadMessages int64  `json:"unreadMessages"`           // total number of unread messages in the folder
}
```

## Rationale

Folders are for organizing user messages.

## Reference Implementation

[https://github.com/mailio/go-mailio-core-modules/blob/main/api/api_folders.go](https://github.com/mailio/go-mailio-core-modules/blob/main/api/api_folders.go)

## Security Considerations

Folders specifically belong the a single user.
All API communication must be conducted over TLS 1.2 or newer protocol.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
