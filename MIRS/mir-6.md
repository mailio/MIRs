---
mir: 6
title: Internal Message Envelope
description: Mailio Internal Message Envelope
author: Igor Rendulic (@igorrendulic)
status: Draft
type: Components
created: 2022-06-04
---

## Abstract

Mailio Internal Message Envelope defines the envelope structure to be used with mailio API and internal storage.

## Motivation

Mailio internal data MUST be prepared for:

- no single point of control
- easy accessibility
- unified export
- unified import
- encrypted data
- plain readable data
- transparent support for file services (IPFS, Centralized S3, Arweave...)

## Specification

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

Mailio internal data **MUST** also be sortable by time of receiving/sending.

Mailio internal data structures **MUST** be self-sufficient for encryption/decrytption.

Mailio internal data **MUST** be expandable with additional content types and content sources.

Content **MAY** be encrypted.

All AES keys **MUST** be Sealed using Authenticated Encryption following Secretbox which uses XSalsa20 and Poly1305 to encrypt and authenticate messages with secret-key cryptography. The length of messages is not hidden.

The sealed AES keys **MUST** be interoperable with [NaCl](https://nacl.cr.yp.to/secretbox.html).

Regardless of the originins of the message nor the protocol of the exchange every internal message **MUST** follow the structure as defined:

```go
type ContentType string
type ContentSource string

const (
	ENCRYPTED_HTML ContentType = "enc/html/text" // encrypted HTML content
	MIME           ContentType = "mime"          // MailioMime (normally encrypted and signed on receiving by the mailio server)

	SOURCE_IPFS ContentSource = "ipfs" // content is stored in IPFS
	SOURCE_S3   ContentSource = "s3"   // content is stored in S3
)

// Basic message envelope internal structure (compatible with golang validator)
// In case content type HTML or PLAIN, Both //ContentRef and //AttachmentRefs are in the shape of ContentRef struct
// In case content type MIME, //ContentRef is in the shape of MailioMime struct
type Envelope struct {
	ID                string        `json:"id" validate:"required"`                                   // message id
	SenderPublicKey   string        `json:"senderPublicKey,omitempty"`                                // base64 senders public key
	ReceiverPublicKey string        `json:"receiverPublicKey,omitempty"`                              // base64 receivers public key
	MessageID         string        `json:"messageId" validate:"required,min=3"`                      // RFC2822 message id
	ParentMessageID   string        `json:"parentMessageId,omitempty"`                                // Parent RFC2822 message id
	OwnerAddressHex   string        `json:"ownerAddressHex" validate:"required,len=42"`               // the owner of this message on the current system (usually receivers mailio address in hex format)
	Read              bool          `json:"read"`                                                     // if message has been read by the receiver
	Folder            string        `json:"folder" validate:"required"`                               // folder message resides in
	ContentType       ContentType   `json:"contentType" validate:"required,oneof=enc/html/text mime"` // content type of the message
	ContentRef        *string       `json:"contentRef,omitempty"`                                     // base64 encrypted reference to a content object. Required BoxSharedKey to decrypt
	ContentSource     ContentSource `json:"contentSource,omitempty"`                                  // source of the content (IPFS, AMAZON S3, ...)
	BoxSharedKey      string        `json:"boxSharedKey,omitempty"`                                   // base64 (sealed curve25519+xsalsa20+poly1305 encryption key to unbox content and attachment AES CGM keys. First 24 bytes is nonce)
	AttachmentRefs    []*string     `json:"attachmentRefs,omitempty"`                                 // base64 message attachment references (encrypted pointers to files on a file system. Rquired BoxSharedKey to decrypt)
	Created           int64         `json:"created" validate:"required"`                              // message creation timestamp (unix epoch miliseconds)
	Modified          int64         `json:"modified,omitempty"`                                       // message modification timestamp (unix epoch miliseconds)
}

type ContentRef struct {
	Filename  string `json:"filename,omitempty"`             // file name (optional)
	Hash      string `json:"hash,omitempty"`                 // base64 md5 hash of the file (quick check that file hasn't changed)
	Reference string `json:"reference" validate:"required"`  // required
	Size      int64  `json:"size" validate:"required,min=1"` // file size (at least 1 byte)
	AesKey    string `json:"aesKey,omitempty"`               // base64 (AES GCM key to unlock content)
}
```

`Envelope`

- ID - universally unique id. If key exists on a local system, message should be denied
- SenderPublicKey - base64 encoded senders public key (or mailio server public key)
- ReceiverPublicKey - base64 encoded receivers public key
- MessageID - RFC2822 message id
- ParentMessageID - RFC2822 message id
- OwnerAddressHex - hexadecimal format of the mailio address (extracted from public key)
- Read - if user has read enveloped content, false if not
- Folder - custom string (folder on the local system)
- ContentType - can be encrypted html or a standard Mime message compatible with Mailio Mime object structure
- ContentRef - base64 message (contents may be encrypted with AES GCM)
- ContentSource - string representing type of file storage
- BoxSharedKey - curve25519 XSalsa20 and Poly1305 encrypted and authenticated message containing AES encryption/decryption key. First 24 bytes is nonce
- AttachmentRefs - base64 list of messages (ContentRef)
- Created - creation timestamp in miliseconds since epoch
- Modified - modification timestamp in miliseconds since epoch

`ContentRef`

- Filename - name of the file on the file storage system
- Hash - optional hash (md5) of the file stored on file storage system
- Reference - the link to the file on the corresponding file system
- Size - size of the file stored on the file system
- AesKey - optional AES GCM encryption key in base64 for decrypting a file on a file storage system

## Rationale

The data described in this MIR defines the structure of a single entity stored in local database. Local meaning users browser/computer. Therefor the structures must be interoperable, exportable/importable and potentially encrypted. The description also is sufficient for ordering of the received/sent messages and it's thread like structures.
Encryption is vital part of the messages since eventually they might live on the public file storage system (such as IPFS).Thus systems implementing Mailio internal Envelope must consider which messages are suitable for which file storage system.

## Reference Implementation

[Mailio Message Module](https://github.com/mailio/go-mailio-core-modules/tree/main/message)

## Security Considerations

There is inherent security risk when implementing Mailio Envelope. All content on public file storage systems such as IPFS are available for download to anyone in such networks.

One must consider to encrypt messages when storing them on such networks, unless the messages are purposfully considered to be read by anyone, but owned by a single/multiple entities.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
