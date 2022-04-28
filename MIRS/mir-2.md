---
mir: 2
title: Porting Mailio Cryptolib
author: Igor Rendulic (@igorrendulic)
status: Review
type: Components
created: 2022-04-28
---


## Abstract
Currently Mailio Cryptolib supports only Angular framework. The goal is to have a `npm` package and the library converted to general typescript modular component. 


## Motivation
For Mailio sub systems and open-source community at large to be able to utilize the Mailio cryptography module.   


## Rationale
MAilio Cryptolib is a frontend module. The new Mailio frontend will be "translated" to React. Also, it's not deployed to npm package manager which limits is usage and upgradeability.

## Backwards Compatibility
No requirements for backwards compatibility

## Reference Implementation
[https://github.com/igorrendulic/mailio-cryptolib-angular](https://github.com/igorrendulic/mailio-cryptolib-angular)

## Security Considerations
Mailio Cryptolib is dependent on TweetNacl which has been audited.Check [here](https://github.com/dchest/tweetnacl-js#audits).

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

