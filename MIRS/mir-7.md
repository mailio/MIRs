---
mir: 7
title: Server Capabilities API
description: Server Capabilities API
author: Igor Rendulic (@igorrendulic)
status: Draft
type: API
category: API
created: 2022-07-06
---

## Abstract

Server capabilities API is able to communicate all capabilities of the server, such as:

- active API version
- available communication protocols
- maximum message size
- maximum single attachment size

## Motivation

Defining server capabilities is to simplify the way servers communicate among each other and also client to server communication. The main idea is to lower the number of communication glitches.

## Specification

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

Every server **MUST** have at least 2 protocols:

- REST API for client-server interface operations
- PROTO for inner server communication

Protocol **MUST** have a version defined. The versions number **MUST** comply to format of [Semantic versioning three-part version number](https://semver.org/)

Type **MUST** be either `https` or `proto` string.

Purpose of the protocol **MUST** be one of the values: `restapi` or `exchange`

`MaximumMessageSizeBytes` and `MaximumSingleAttachmentSizeBytes` must be size defined in bytes. Messages that exceed the maximum sizes **MAY** be accepted but the default behaviour is to expect failure.

```
// ServerCapabilites
type ServerCapabilities struct {
	EmailDomains                     []string          `json:"emailDomains,omitempty"`                                     // supported email domains
	SupportedProtocols               []*ServerProtocol `json:"supportedProtocols" validate:"required,min=2"`               // list of supported protocols (at least REST and Proto protocols)
	MaximumMessageSizeBytes          int64             `json:"maximumMessageSizeBytes" validate:"required,min=1"`          // maximum message size in bytes
	MaximumSingleAttachmentSizeBytes int64             `json:"maximumSingleAttachmentSizeBytes" validate:"required,min=1"` // maximum single attachment size in bytes
}

// Protocol description, purpose and version
type ServerProtocol struct {
	Name    string `json:"name,omitempty"`                                     // name of the protocol
	Version string `json:"version" validate:"required"`                        // version of the protocol
	Type    string `json:"type" validate:"required,oneof=https proto"`         // type of the protocol
	Purpose string `json:"purpose" validate:"required,oneof=restapi exchange"` // purpose of the protocol
}
```

`ServerCapabilities`

- EmailDomains - list of supported email domains (e.g. [mail.io]), optional
- SupportedProtocols - list of supported protocols.

## Rationale

The server capabilities are meant to lower the communication glitches for server to server and client to server communications.

## Backwards Compatibility

Versioning of protocols is essential for backwards compatibility. Servers **MUST** be able to support all previous versions of implemented protocols.

## Reference Implementation

[https://github.com/mailio/go-mailio-core-modules/blob/main/api/api.go](https://github.com/mailio/go-mailio-core-modules/blob/main/api/api.go)

## Security Considerations

All protocols must have end-to-end encrypted communication.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
