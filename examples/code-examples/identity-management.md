# Identity management

## Introduction <a id="manage-identities"></a>

Let's review some code examples on how to manage **Identities**:

* ​[Creating a remote identity](https://docs.radixdlt.com/alpha/developer/javascript-client-library-guide/code-examples#creating-a-remote-identity)​
* ​[Creating a simple identity](https://docs.radixdlt.com/alpha/developer/javascript-client-library-guide/code-examples#creating-a-simple-identity)​

{% hint style="success" %}
**Tip:** using a _RemoteIdentity_ allows the JavaScript application to access an existing account that the user already has on a Wallet, while keeping the private keys secure.
{% endhint %}

## Creating a simple identity

{% hint style="info" %}
**Note:** a _SimpleIdentity_ keeps both public and private keys in memory.
{% endhint %}

In the following code example, we create a new simple identity using the `RadixIdentityManager`'s generateSimpleIdentity\(\) method:

```javascript
const identityManager = new RadixIdentityManager()
const myIdentity = identityManager.generateSimpleIdentity()  
```

With it, we can easily access our own Account using the `account` reference:

```javascript
const myAccount = myIdentity.account​ ​ 
console.log('My account address: ', myAccount.getAddress())
```

## Creating a remote identity

{% hint style="info" %}
**Note:** a _RemoteIdentity_ only holds the public key in memory, while a Wallet keeps the private key. The user must accept a request from the JavaScript application to allow the Wallet to sign atoms on its behalf.
{% endhint %}

In the following code snippet, we try to create a new remote **Identity** and catch any errors in the console log:

```javascript
RadixRemoteIdentity.createNew('my dApp', 'my dApp description').then(identity => {
    //Do stuff with identity here
}).catch(error => {
    console.log(error)
})
```

We can also specify a remote host and port for the remote identity:

```javascript
RadixRemoteIdentity.createNew('my dApp', 'my dApp description', '192.168.0.123', '53433').then(identity => {
    //Do stuff with identity here
}).catch(error => {
    console.log(error)
})
```

In case we want to validate if the remote wallet is up and running:

```javascript
RadixRemoteIdentity.isServerUp().then(isServerUp => {
    if (isServerUp) {
         // Do something
    }
})
```

As an alternative, we can also generate a _RemoteIdentity_ using `RadixIdentityManager`:

```javascript
const identityManager = new RadixIdentityManager()
​
identityManager.generateRemoteIdentity('my dApp', 'my dApp description').then(identity => {
    //Do stuff with identity here
}).catch(error => {
    console.log(error)
})
```

