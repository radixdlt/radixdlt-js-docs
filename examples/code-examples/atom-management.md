# Atom management

## Introduction

Let's review some code examples on how to manage **Atoms**:

* [Reading atoms from a public address](https://docs.radixdlt.com/alpha/developer/javascript-client-library-guide/code-examples#reading-atoms-from-a-public-address)
* [Reading and decrypting atoms from an owned address](https://docs.radixdlt.com/alpha/developer/javascript-client-library-guide/code-examples#reading-and-decrypting-atoms-from-an-owned-address)
* [Caching atoms](https://docs.radixdlt.com/alpha/developer/javascript-client-library-guide/code-examples#caching-atoms)

## Reading atoms from a public address

In the following code snippet we read **Atoms** from the public address `JEaSfBftmFdseSRKfTm6hJNhQi3FAEqmVzAPTxsf55wPqXvBxRB`, by opening a **Node** connection and subscribing to the transaction updates.

```javascript
const account = RadixAccount.fromAddress('JEaSfBftmFdseSRKfTm6hJNhQi3FAEqmVzAPTxsf55wPqXvBxRB')
account.openNodeConnection()

account.transferSystem.balance // This is the account balance
account.transferSystem.tokenUnitsBalance // This is the account balance in token units
account.transferSystem.transactions // This is a list of transactions 

// Subscribe for any new incoming transactions
account.transferSystem.transactionSubject.subscribe(transactionUpdate => {
  console.log(transactionUpdate)
})

// Subscribe for all previous transactions as well as new ones
account.transferSystem.getAllTransactions().subscribe(transactionUpdate => {
  console.log(transactionUpdate)
})
```

## Reading and decrypting atoms from an owned address

In the following code snippet we read and decrypt **Atoms** from an owned address, by opening a **Node** connection and getting the application data from `my-test-application`.

```javascript
const identityManager = new RadixIdentityManager()
const identity = identityManager.generateSimpleIdentity()

// Each identity comes with an account, which works the same as any account, but can also decrypt encrypted messages
const account = identity.account

account.openNodeConnection() 

// A list of Radix chat messages in the order they're received
account.messagingSystem.messages 

// Radix chat messages grouped by the other address
account.messagingSystem.chats 

// Subscribe for incoming messages
account.messagingSystem.messageSubject.subscribe(...)

// Subscribe for all previous messages as well as new ones
account.messagingSystem.getAllMessages().subscribe(...)

// Custom application data 
account.dataSystem.applicationData.get('my-test-application')

// Subscribe for all incoming application data
account.dataSystem.applicationDataSubject.subscribe(...)

// Subscribe for all previous messages as well as new ones
account.dataSystem.getApplicationData('my-test-application').subscribe(...)

// Subscribe for all previous messages as well as new ones signed by specific addresses
account.dataSystem.getApplicationData('my-test-application', ['JEaSfBftmFdseSRKfTm6hJNhQi3FAEqmVzAPTxsf55wPqXvBxRB']).subscribe(...)
```

## Caching atoms

In the following code snippet we cache **Atoms** from the public address `JEaSfBftmFdseSRKfTm6hJNhQi3FAEqmVzAPTxsf55wPqXvBxRB`, by defining a `'path/to/file'` and enabling the account's cache.

```javascript
import {RadixNEDBAtomCache} from 'radixdlt'

const account = RadixAccount.fromAddress('JEaSfBftmFdseSRKfTm6hJNhQi3FAEqmVzAPTxsf55wPqXvBxRB')
const atomCache = new RadixNEDBAtomCache('path/to/file')
account.enableCache(atomCache) // This will read all atoms in cache, as well as store new ones in the future

account.openNodeConnection()
```

