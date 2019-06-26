# Private key management

## Introduction <a id="manage-private-keys"></a>

Let's review the following code examples on how to store and load private keys:

* ​[Storing private keys](private-key-management.md#storing-private-keys)​
* ​[Loading private keys](private-key-management.md#loading-private-keys)​

## Storing private keys

In the following code snippet we encrypt the private key of an identity using the password `SuperDuperSecretPassword`. The resulting JSON object can be stored to a file, or in a browser's local storage.

```javascript
const identity = identityManager.generateSimpleIdentity()
​
const password = 'SuperDuperSecretPassword'
RadixKeyStore.encryptKey(identity.keyPair, password).then((encryptedKey) => {
  console.log('Private key encrypted')
}).catch((error) => {
  console.error('Error encrypting private key', error)
})
```

{% hint style="info" %}
The key storage format is compatible with the Radix Desktop and Android wallet applications.
{% endhint %}

## Loading private keys

In the following code snippet we decrypt a private key using `SuperDuperSecretPassword` as the decryption password.

```javascript
const encryptedKey = loadKeyFromStorage() // This object is what you get from RadixKeyStore.encryptKey(...)
const password = 'SuperDuperSecretPassword'
RadixKeyStore.decryptKey(encryptedKey, password).then((keyPair) => {
  console.log('Private key successfuly decrypted')
​
  const identity = new RadixSimpleIdentity(keyPair)
}).catch((error) => {
  console.error('Error decrypting private key', error)
})
```

