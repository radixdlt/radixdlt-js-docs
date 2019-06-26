# General use

## Introduction  <a id="manage-identities"></a>

Let's review some code examples on how to handle generic tasks:

* [Initializing a universe](https://docs.radixdlt.com/alpha/developer/javascript-client-library-guide/code-examples#initializing-a-universe)
* [Setting a log level](https://docs.radixdlt.com/alpha/developer/javascript-client-library-guide/code-examples#setting-a-log-level)

## Initializing a universe

There are different Universes available, such as `LOCAL`, `SUNSTONE`, and `BETANET`. Typically, for development purposes we use **BETANET**.

To bootstrap to a network we call:

```javascript
radixUniverse.bootstrap(RadixUniverse.BETANET)
```

## Setting a log level

In the following code snippet we set the log level to display errors only:

```javascript
import { RadixLogger } from 'radixdlt' RadixLogger.setLevel('error')
```

{% hint style="info" %}
Possible log level values are: `trace`, `debug`, `info`, `warn`, `error`.
{% endhint %}

