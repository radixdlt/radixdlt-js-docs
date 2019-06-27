# Saving dApp settings

## Introduction <a id="introduction"></a>

In this short tutorial you'll learn how a distributed App \(dApp\) can save and load settings directly to the Radix distributed ledger using our [JavaScript library](../).

{% hint style="success" %}
**Tip:** If you're new to our JavaScript library, we recommend reading [this guide](get-started.md).
{% endhint %}

## Manage settings <a id="manage-settings"></a>

Loading and saving settings are fundamental activities in the workflow of any software application. When we are building a dApp, we can take advantage of the distributed ledger and store those settings directly on it. In this section we present basic JavaScript examples to **Save** and **Load** settings to the Radix ledger.

### Saving <a id="saving"></a>

Before we start, your application needs to have an [**identity**](https://docs.radixdlt.com/alpha/learn/glossary#identity) in the ledger. Let's assume that your dApp has an identity `appIdentity` already available, and that we have the following settings to save:

```javascript
const mySettings = {
	user: 'user@example.com',
	name: 'John Connor',
	verified: true,
	version: 12345
}
```

{% hint style="info" %}
**Note:** check our [code examples](../examples/code-examples/) articles to learn how to [create identities](../examples/code-examples/identity-management.md#creating-a-simple-identity).
{% endhint %}

To save the application settings to the ledger, you have to...

* open a [**node**](https://docs.radixdlt.com/alpha/learn/glossary#nodes) connection to the network,
* prepare the payload \(`appSettings`\) by encoding `mySettings`,
* build a transaction using `RadixTransactionBuilder`,
* sign and submit the transaction with the `appIdentity` identity.

{% hint style="info" %}
**Note:** you can also subscribe to get transaction status updates and validate if settings were saved correctly.
{% endhint %}

```javascript
// appIdentity is the application Identity
const myAccount = appIdentity.account
​
myAccount.openNodeConnection()
// Wait for the account to sync data from the ledger
​
const applicationId = 'my-test-app.settings'
const appSettings = JSON.stringify(mySettings)
​
const transactionStatus = RadixTransactionBuilder
 .createPayloadAtom([myAccount], applicationId, appSettings)
 .signAndSubmit(appIdentity)
                  
transactionStatus.subscribe({
 complete: () => { console.log('Settings saved successfully.') },
 error: error => { console.error('Error saving settings!', error) }
})
```

### Loading <a id="loading"></a>

As in the previous example, let's assume that your dApp has an identity `appIdentity` already available. Loading settings from the ledger is simple and straight forward. To load the application settings from the ledger, you have to...

* open a [**node**](https://docs.radixdlt.com/alpha/learn/glossary#nodes) connection to the network,
* get the [**address**](https://docs.radixdlt.com/alpha/learn/glossary#address) of the application account
* subscribe to the account **data system** to get all the messages signed by the application

```javascript
myAccount.openNodeConnection()
// Wait for the account to sync data from the ledger
​
const myAppAddress = myAccount.getAddress()
​
// Subscribe for all previous messages as well as new ones signed by a specific address
myAccount.dataSystem.getApplicationData('my-test-app.settings', [myAppAddress]).subscribe(
 appData => { console.log(appData.data.payload)
 })
```

## Complete example <a id="complete-example"></a>

To wrap up this tutorial, we bring the [saving](saving-dapp-settings.md#saving) and [loading](saving-dapp-settings.md#loading) pieces together and present this example dApp that saves user settings to the ledger.

```javascript
import {radixUniverse, RadixUniverse} from 'radixdlt'
import {RadixIdentityManager, RadixTransactionBuilder} from 'radixdlt'
​
radixUniverse.bootstrap(RadixUniverse.BETANET)
​
const identityManager = new RadixIdentityManager()
const appIdentity = identityManager.generateSimpleIdentity()
const myAccount = appIdentity.account
​
myAccount.openNodeConnection()
// Wait for the account to sync data from the ledger
​
const applicationId = 'my-test-app.settings'
const appSettings = JSON.stringify({
 user: 'user@example.com',
 name: 'John Connor',
 verified: true,
 version: 12345
})
​
const transactionStatus = RadixTransactionBuilder
 .createPayloadAtom([myAccount], applicationId, appSettings)
 .signAndSubmit(appIdentity)
                  
transactionStatus.subscribe({
 complete: () => { console.log('Settings saved successfully.') },
 error: error => { console.error('Error saving settings!', error) }
})
​
const myAppAddress = myAccount.getAddress()
​
// Subscribe for all previous messages as well as new ones signed by a specific address
myAccount.dataSystem.getApplicationData('my-test-app.settings', [myAppAddress]).subscribe(
 appData => { console.log(appData.data.payload)
 })
```

