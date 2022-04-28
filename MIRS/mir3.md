---
mir: 3
title: Porting Mailio Evaporate
author: Igor Rendulic (@igorrendulic)
status: Review
type: Components
created: 2022-04-28
---


## Abstract
Currently Mailio Evaporate supports only Angular framework. The goal is to have a `npm` package and the library converted to general typescript modular component. 


## Motivation
For Mailio sub systems and open-source community at large to be able to utilize the Mailio evaporate module.   


## Rationale
Mailio Evaporate is a frontend module. The new Mailio frontend will be "translated" to React. Also, it's not deployed to npm package manager which limits is usage and upgradeability.

## Backwards Compatibility
No requirements for backwards compatibility

## Reference Implementation
[https://github.com/igorrendulic/mailio-evaporate-angular](https://github.com/igorrendulic/mailio-evaporate-angular)

## Security Considerations
Dependencies need to be upgraded during the port.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

