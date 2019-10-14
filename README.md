---
description: >-
  The developers guide to building on the Radix Network with the JavaScript
  client library
---

# JavaScript client library

[![License MIT](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/radixdlt/radixdlt-js/blob/master/LICENSE) [![Build Status](https://travis-ci.com/radixdlt/radixdlt-js.svg?branch=develop)](https://travis-ci.com/radixdlt/radixdlt-js) [![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=radixdlt-js&metric=alert_status)](https://sonarcloud.io/dashboard?id=radixdlt-js) [![Reliability](https://sonarcloud.io/api/project_badges/measure?project=radixdlt-js&metric=reliability_rating)](https://sonarcloud.io/component_measures?id=radixdlt-js&metric=reliability_rating) [![Security](https://sonarcloud.io/api/project_badges/measure?project=radixdlt-js&metric=security_rating)](https://sonarcloud.io/component_measures?id=radixdlt-js&metric=security_rating) [![Code Corevage](https://sonarcloud.io/api/project_badges/measure?project=radixdlt-js&metric=coverage)](https://sonarcloud.io/component_measures?id=radixdlt-js&metric=Coverage)

## Introduction

A JavaScript client library for interacting with a [Radix](https://www.radixdlt.com/) Distributed Ledger.

{% hint style="success" %}
**Tip:** for an overview of the main components of the library and how they fit together, read this [blog post](https://www.radixdlt.com/post/introducing-the-radix-javascript-library).
{% endhint %}

{% hint style="info" %}
**Note:** this library and the network itself are currently in **Beta** development phase. Please report any issues in the [GitHub issue tracker](https://github.com/radixdlt/radixdlt-js/issues).
{% endhint %}

## Features

* Full TypeScript support
* Follow the reactive programming pattern using [RxJS](https://rxjs-dev.firebaseapp.com/)​
* Cryptography using the [elliptic](https://github.com/indutny/elliptic) library
* Automatically manage connection to the Radix [Universe](https://docs.radixdlt.com/alpha/learn/glossary#universe) in a sharded environment
* Communication with the Radix network usign RPC over WebSockets
* Read [Atoms](https://docs.radixdlt.com/alpha/learn/glossary#atoms) in any address
* Write [Atoms](https://docs.radixdlt.com/alpha/learn/glossary#atoms) to the ledger
* End-to-end data encryption using ECIES

## Installation

To install the library in your own project using [yarn package manager](https://yarnpkg.com/):

```bash
yarn add radixdlt
```

## Example applications

* ​​[Express.js server example](https://github.com/radixdlt/radixdlt-js-server-example)​

## Build

To build the library using your preferred package manager, run:

```bash
yarn install && yarn build
```

### Test

Run tests with `yarn test`.

## Known issues

### Angular 6+

```text
Error: Can't resolve 'crypto'
```

On Angular 6+ versions, the node module polyfills from webpack are not bundled. To fix your issue with crypto, path, etc. use the fix described in [this answer](https://github.com/angular/angular-cli/issues/1548#issuecomment-427653778).

## License

The `radixdlt-js` library is released under the [MIT License](https://github.com/radixdlt/radixdlt-js/blob/master/LICENSE).

## Join the Radix Community  <a id="join-the-radix-community"></a>

* ​[Telegram](https://t.me/radix_dlt) for general chat
* ​[Discord](https://discord.gg/7Q7HSZZ) for developer chat
* ​[Reddit](https://reddit.com/r/radix) for general discussion
* ​[Forum](https://forum.radixdlt.com/) for technical discussion
* ​[Twitter](https://twitter.com/radixdlt) for announcements
* ​[Email newsletter](https://radixdlt.typeform.com/to/nyKvMV) for weekly updates
* Mail to [hello@radixdlt.com](mailto:info@radixdlt.com) for general enquiries.

