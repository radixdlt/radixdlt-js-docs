---
description: >-
  The developers guide to building on the Radix Network with the JavaScript
  client library
---

# JavaScript client library

## Introduction

A JavaScript client library for interacting with a [Radix](https://www.radixdlt.com/) Distributed Ledger.

{% hint style="success" %}
**Tip:** for an overview of the main components of the library and how they fit together, read this [blog post](https://www.radixdlt.com/post/introducing-the-radix-javascript-library).
{% endhint %}

{% hint style="info" %}
**Note:** this library and the network itself are currently in **Alpha** development phase. Please report any issues in the [GitHub issue tracker](https://github.com/radixdlt/radixdlt-js/issues).
{% endhint %}

## Features

* Full Typescript support
* Follow the reactive programming pattern using [RxJS](https://rxjs-dev.firebaseapp.com/)​
* Cryptography using the [elliptic](https://github.com/indutny/elliptic) library
* Automatically manage connection to the Radix [Universe](https://docs.radixdlt.com/alpha/learn/glossary#universe) in a sharded environment
* Communication with the Radix network usign RPC over WebSockets
* Read [Atoms](https://docs.radixdlt.com/alpha/learn/glossary#atoms) in any address
* Write [Atoms](https://docs.radixdlt.com/alpha/learn/glossary#atoms) to the ledger
* End-to-end data encryption using ECIES

## Installation

To install the library using your preferred package manager, run:

```bash
yarn add radixdlt
```

or

```bash
npm install radixdlt --save
```

## Example applications

* ​[Front-end example using Vue.js](https://github.com/radixdlt/radixdlt-js-skeleton)​
* ​[Express.js server example](https://github.com/radixdlt/radixdlt-js-server-example)​

## Build

To build the library using your preferred package manager, run:

```bash
yarn install && yarn build
```

or

```bash
npm install && npm build
```

### Test

Run tests with `yarn test`.

## Known issues

### Angular

Apparently on Angular 6+ versions, the node module polyfills from webpack are not bundled. To fix your issue with crypto, path, etc. go to `node_modules/@angular-devkit/build-angular/src/angular-cli-files/models/webpack-configs/browser.js` and do the following change:

```javascript
node: { crypto: true, path: true }
```

{% hint style="warning" %}
**Note:** this is not a reproducible fix. If you install your modules in a new location, you will lose this change.
{% endhint %}

## Join the Radix Community <a id="join-the-radix-community"></a>

* ​[Telegram](https://t.me/radix_dlt) for general chat
* ​[Discord](https://discord.gg/7Q7HSZZ) for developer chat
* ​[Reddit](https://reddit.com/r/radix) for general discussion
* ​[Forum](https://forum.radixdlt.com/) for technical discussion
* ​[Twitter](https://twitter.com/radixdlt) for announcements
* ​[Email newsletter](https://radixdlt.typeform.com/to/nyKvMV) for weekly updates
* Mail to [hello@radixdlt.com](mailto:info@radixdlt.com) for general enquiries.

