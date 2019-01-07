# Getting started

This tutorial is written for people who prefer to learn by doing. You might find this tutorial and the [GitHub wiki](https://github.com/radixdlt/radixdlt-js) complementary to each other.

## Introduction <a id="introduction"></a>

During this tutorial, we will build a small distributed App \(dApp\). The techniques you’ll learn in this guide are fundamental to building any dApp on [Radix](http://www.radixdlt.com/), and mastering it will give you a better understanding of Radix distributed ledger.

This guide is divided into several sections:

* **​**[**Basic Setup**](get-started.md#basic-setup) will give you a starting point to follow the tutorial.
* **​**[**Tech overview**](get-started.md#overview) will teach you the fundamentals of Radix's architecture.
* **​**[**Getting some Radix tokens**](get-started.md#getting-some-radix-test-tokens) will show you how to build your first basic dApp.
* **​**[**Beyond the basics**](get-started.md#beyond-the-basics) will give you additional examples to get a deeper insight into the strengths of Radix JS library.

### About our example dApp <a id="about-our-example-dapp"></a>

Our example dApp for this guide will get some free test money from Radix, and once it has enough money, our dApp will send some to a different address. With our small dApp you'll learn how to interact with accounts, send transactions across the network and handle test tokens in an easy way.

Don't worry if you're new to Radix's concepts, as we will review the basic building blocks along the way.

## Basic setup <a id="basic-setup"></a>

Our next step is to set you up so that you can start building your first dApp with Radix.

### Prepare your environment <a id="prepare-your-environment"></a>

Before we can begin, make sure you have a recent version of [Node.js](https://nodejs.org/) installed in your system. We'll also need a minimalistic Node.js boilerplate project so we can properly build and run our dApp. If you're experienced building your own Node.js applications, feel free to skip our next step and continue with the library [installation](get-started.md#installation).

#### Setting up a simple Node.js project <a id="setting-up-a-simple-node-js-project"></a>

To prepare the environment, let's now set up a minimalistic Node.js project, by cloning an open source [Webpack ES6 boilerplate](https://www.npmjs.com/package/webpack-es6-boilerplate):

```bash
git clone https://github.com/jluccisano/webpack-es6-boilerplate.git
```

Once we have cloned the boilerplate project, we can go ahead and install the required libraries for it:

```bash
cd webpack-es6-boilerplate/
npm install
```

As an optional step, you can start the server using `npm start` and open [http://localhost:9000](http://localhost:9000/) on your browser to see if everything was set up correctly.

### Installation <a id="installation"></a>

You can install the _**radixdlt**_ library in your Node.js project using your preferred package manager:

#### Npm <a id="npm"></a>

```bash
npm install radixdlt --save
```

#### Yarn <a id="yarn"></a>

```bash
yarn add radixdlt
```

{% hint style="info" %}
**Note:** the radixdlt library provides full TypeScript support
{% endhint %}

### Recommendations <a id="recommendations"></a>

For this guide, we will assume that you have some familiarity with JavaScript, but you should be able to follow along even if you’re coming from a different programming language.

{% hint style="success" %}
If you need to review JavaScript, we recommend reading [this guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript).
{% endhint %}

We’ll also assume that you’re familiar with programming concepts like functions, objects, arrays, and classes. Additionally, having an understanding of Reactive programming, RxJS and observable patterns is suggested for a better insight.

{% hint style="success" %}
If you need a quick review on Reactive programming, we recommend reading [our blog post](https://www.radixdlt.com/post/reactive-programming-and-rxjs).
{% endhint %}

## Tech overview <a id="overview"></a>

Now that you’re set up, let’s dig a bit on the concepts that make Radix a unique distributed ledger technology, so we can share a common language.

/// --- ///

## Getting some Radix test tokens <a id="getting-some-radix-test-tokens"></a>

Now that we have done a brief overview of the concepts behind Radix and we share a common language, we are ready to begin building our example dApp and get some _**Radix test tokens**_ along the way.

### Initializing the Universe <a id="initializing-the-universe"></a>

The first step, before we can interact with the ledger, is to choose which [**Universe**](../radix-concepts.md#universe) we want to connect to. We will use the **ALPHANET** universe configuration since it's our main testing environment, and the other development universes are used for testing unstable features.

Now, to initialize the universe we have to import the _radixUniverse_ singleton from the library, and call the bootstrap function with the _ALPHANET_ universe configuration:

```javascript
import {radixUniverse, RadixUniverse} from 'radixdlt'

​​radixUniverse.bootstrap(RadixUniverse.ALPHANET)
```

### Creating our own Identity <a id="creating-our-own-identity"></a>

As we want to interact with the ledger and be able to sign and decrypt atoms, we'll need to have our own [**Identity**](../radix-concepts.md#identity). To create a new random identity, we use the `RadixIdentityManager`:

```javascript
const identityManager = new RadixIdentityManager()
```

Now we create a new random identity using the Identity Manager's _generateSimpleIdentity\(\)_ method:

```javascript
const myIdentity = identityManager.generateSimpleIdentity()
```

With it, we can easily get our own [**Account**](../radix-concepts.md#account) using the _account_ reference:

```javascript
const myAccount = myIdentity.account​​

console.log('My account address: ', myAccount.getAddress())
```

{% hint style="info" %}
Each [**Identity**](../radix-concepts.md#identity) automatically comes with a corresponding [**Account**](../radix-concepts.md#account).
{% endhint %}

### Opening the connection <a id="opening-the-connection"></a>

Now that we have our account, the next step is connect it to the network. We do it by calling the _openNodeConnection\(\)_ method:

```javascript
myAccount.openNodeConnection()
```

This call opens a connection to a [**Node**](../radix-concepts.md#nodes) from the _ALPHANET_ universe which serves the shard where our account lives on, and asks the node for all the atoms in the address. It will also maintain a connection to the network until we destroy the account.

{% hint style="success" %}
**Tip:** if a connection to a node dies, a new one will be found automatically.
{% endhint %}

### Getting the Faucet's account <a id="getting-the-faucets-account"></a>

To get the [**Faucet's**](../radix-concepts.md#faucet-service) account, we resolve the address using the _fromAddress\(...\)_ method:

```javascript
const faucetAddress = '9ey8A461d9hLUVXh7CgbYhfmqFzjzSBKHvPC8SMjccRDbkTs2aM'​​

const faucetAccount = RadixAccount.fromAddress(faucetAddress, true)
```

Regarding the second `boolean` parameter, we set it to _true_, so the method doesn't create the default Account systems along the way.

{% hint style="success" %}
For accounts that won't be connected to the network, since you won't need any accounting system, set the parameter to _true_.
{% endhint %}

{% hint style="warning" %}
**Note:** the library will throw an error if you accidentally try to use an address from a different **Universe**.
{% endhint %}

### Sending a message to the Faucet <a id="sending-a-message-to-the-faucet"></a>

Now that we have the Faucet's account and our own account connected to the network, we are ready to send a message and request some free tokens to the Faucet service. We send the message using RadixTransactionBuilder's _createRadixMessageAtom\(...\)_ method, and signing the result [**Atom**](../radix-concepts.md#atoms) with our [**Identity**](../radix-concepts.md#identity):

```javascript
const message = 'Dear Faucet, may I please have some money? (◕ᴥ◕)'​
​
RadixTransactionBuilder
  .createRadixMessageAtom(myAccount, faucetAccount, message)
  .signAndSubmit(myIdentity)
```

### Subscribing to balance updates <a id="subscribing-to-balance-updates"></a>

After we send the message, we have to subscribe to the Balance subject from the Transfer system to know when we receive the free test tokens sent by the [**Faucet**](../radix-concepts.md#faucet-service) service:

```javascript
myAccount.transferSystem.balanceSubject.subscribe(balance => {
  // there's a balance update
  console.log(balance);
  // ...
})
```

{% hint style="info" %}
The _balance_ includes the type of token, and the amount of tokens in subunits.
{% endhint %}

### Handling tokens in Radix <a id="handling-tokens-in-radix"></a>

As the balance can have different types of tokens, let's see now how we handle tokens in Radix using the _RadixTokenManager_. Since we are working with the free test tokens, first we get the specific RadixToken by calling the _getTokenbyISO\(...\)_ method from `RadixTokenManager`:

```javascript
const radixToken = radixTokenManager.getTokenByISO('TEST')
```

We also noted that the balance is stored as token subunits \(integer values\), but we want to work with the balance as a regular floating point number, so we convert it using the _toTokenUnits\(...\)_ method from RadixToken:

```javascript
// Convert balance from subunits to decimal point value
const floatingPointBalance = radixToken.toTokenUnits(balance[radixToken.id.toString()])
```

{% hint style="info" %}
**Note:** we convert token subunits to floating point on-demand to avoid inaccurate floating point calculations.
{% endhint %}

### Sending Radix tokens <a id="sending-radix-tokens"></a>

Our last step is to send some of the free test tokens that we've got from the Faucet, to another address on the network. To do it, first we get the account from the destination address, just as we did before for the Faucet service:

```javascript
// Put your friends' address here
const toAddress = '9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM'
const toAccount = RadixAccount.fromAddress(toAddress, true)
```

We now have the destination account and our own account connected to the network, and we are ready to send a few test tokens to our friend's account.  
We send the tokens using RadixTransactionBuilder's _createTransferAtom\(...\)_ method, and signing the result Atom with our Identity:

```javascript
// Send 5 tokens to the address
RadixTransactionBuilder
  .createTransferAtom(myAccount, toAccount, radixToken, 5)
  .signAndSubmit(myIdentity)
```

### The complete dApp <a id="the-complete-dapp"></a>

At this point, we have all the basic building blocks for our simple "Get Radix test tokens" dApp. Now, to have a real complete and functional dApp, we need to put the pieces together:

```javascript
import {radixUniverse, RadixUniverse} from 'radixdlt'
import {RadixIdentityManager, RadixAccount} from 'radixdlt'
import {RadixTransactionBuilder, radixTokenManager} from 'radixdlt'
​
radixUniverse.bootstrap(RadixUniverse.ALPHANET)
​
const identityManager = new RadixIdentityManager()
const myIdentity = identityManager.generateSimpleIdentity()
const myAccount = myIdentity.account
​
myAccount.openNodeConnection()
​
const faucetAddress = '9ey8A461d9hLUVXh7CgbYhfmqFzjzSBKHvPC8SMjccRDbkTs2aM'
const faucetAccount = RadixAccount.fromAddress(faucetAddress, true)
const message = 'Dear Faucet, may I please have some money? (◕ᴥ◕)'
​
RadixTransactionBuilder
  .createRadixMessageAtom(myAccount, faucetAccount, message)
  .signAndSubmit(myIdentity)
  
  
const radixToken = radixTokenManager.getTokenByISO('TEST')  
myAccount.transferSystem.balanceSubject.subscribe(balance => {
  // Convert balance from subunits to decimal point value
  const floatingPointBalance = radixToken.toTokenUnits(balance[radixToken.id.toString()])
  // do we have at least 5 tokens?
  if (floatingPointBalance > 5) {
  
    // Put your friends' address here
    const toAddress = '9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM'
    const toAccount = RadixAccount.fromAddress(toAddress, true)
    
    // Send 5 tokens to the address
    RadixTransactionBuilder
      .createTransferAtom(myAccount, toAccount, radixToken, 5)
      .signAndSubmit(myIdentity)
  }
})
```

Finally, to see this code running, we have to include it in our Node.js project. If you're using the [minimalistic boilerplate](https://www.npmjs.com/package/webpack-es6-boilerplate) project that we [set up previously](get-started.md#setting-up-a-simple-node-js-project), you only have to copy the dApp code shown above to the `./src/index.js` file. You can also add the following line at the end of the file to get your Radix address in the web browser window:

```javascript
document.getElementById('root').innerHTML = 'My address: '+ myAccount.getAddress();
```

To run the dApp, use `npm start`, and point your browser to [http://127.0.0.1:9000](http://127.0.0.1:9000/). You should see your address on the main window, and if you open the developer console you will see the messages and atoms flowing through the network:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LMPbV3hGbTEzGtYlH-m%2F-LPeLHn0knYfHDq1cHWh%2F-LPePT4SoHkB1ViOip-V%2FScreenshot%202018-10-24%2013.52.26.png?alt=media&token=80810e01-45be-4644-823d-8cbe5abaa1d6)

## Beyond the basics <a id="beyond-the-basics"></a>

As we reach the end of our dApp example, we want to share some extra code snippets for those who want to go beyond the basics and showcase a few additional things that you can do with our library.

### Example applications <a id="example-applications"></a>

* ​[Front-end example using Vue.js](https://github.com/radixdlt/radixdlt-js-skeleton)​
* ​[Express.js server example](https://github.com/radixdlt/radixdlt-js-server-example)​

### Code examples <a id="code-examples"></a>

* [Manage accounts​](../examples/code-examples/manage-accounts.md)
* [Manage atoms​](../examples/code-examples/manage-atoms.md)
* [Manage identities](../examples/code-examples/identity-management.md)
* ​[Manage transactions​](../examples/code-examples/transaction-management.md)
* ​[Manage private keys​](../examples/code-examples/private-key-management.md)

