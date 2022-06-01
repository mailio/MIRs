---
mir: 5
title: DNS Discovery
author: Igor Rendulic (@igorrendulic)
status: Draft
type: Components
created: 2022-06-01
---

## Abstract

Mailio needs to distinguish between ordinary SMTP able server and Mailio able server. It also needs to exchange public keys to prevent possible security issues (mail spoofing), alternate allowed message senders and verifiability of the exchanged messages.

## Motivation

Mailio needs to recognize other Mailio systems in order to successfully exchange messages.

## Specification

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in [RFC-2119](https://www.ietf.org/rfc/rfc2119.txt).

### Verifiability

> Mailio senders must have a way to verify/authenticate the receiving messages.

Mailio server **MUST** have a DNS TXT record that **MUST** adhere to the format defined in this section.

`mailio public exchange key`

```
mailio._mailiokey.mail.io 	TXT	"v=MAILIO1; k=ed25519; p=AAAAC3NzaC1lZDI1NTE5AAAAIBfJ2Qjt5GPi7DKRPGxJCkvk8xNsG9dA607tnWagOk2D"
```

The structure of a MAILIO DNS TXT record **MUST** be `<selector>._mailiokey.<domain>`, where the selector **MUST** be `mailio`.

`p=AAA...` **MUST** be the public Mailio Ed25519 signature key in `base64` format. The Mailio public key **MUST** be 32 bytes in length.

The DNS TXT record **MUST** always start with verision: `v=MAILIO1`.

> The structure is insipired by the DKIM DNS TXT record. Currently only version MAILIO1 will be supported.

## Security Considerations

- Sender/Receiver signature verification

The current specification does not support routing messages via alternate servers.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
