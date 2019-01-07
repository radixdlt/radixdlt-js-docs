# Transaction management

## Introduction <a id="manage-transactions"></a>

Let's review a few code examples related to `RadixTransactionBuilder`, and see how we can handle transactions, messages and payloads:

* ​[Sending a transaction](https://docs.radixdlt.com/alpha/developer/javascript-client-library-guide/code-examples#sending-a-transaction)​
* ​[Sending a message](https://docs.radixdlt.com/alpha/developer/javascript-client-library-guide/code-examples#sending-a-message)​
* ​[Storing an application payload](https://docs.radixdlt.com/alpha/developer/javascript-client-library-guide/code-examples#storing-an-application-payload)​

## Sending a transaction

In the following code snippet we send a transaction from an owned address to the public address `9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM`, by creating a transfer **Atom** and signing it with our **Identity**. Finally we get the results by subscribing to the transaction updates.

```javascript
const myIdentity = identityManager.generateSimpleIdentity()
const myAccount = myIdentity.account
myAccount.openNodeConnection()
​
// Wait for the account to sync data from the ledger
​
// No need to load data from the ledger for the recipient account
const toAccount = RadixAccount.fromAddress('9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM', true)
const token = 'TEST' // The Radix TEST token
const amount = 123.12
​
const transactionStatus = RadixTransactionBuilder
  .createTransferAtom(myAccount, toAccount, token, amount)
  .signAndSubmit(myIdentity)
                    
transactionStatus.subscribe({
  next: status => {
    console.log(status) 
    // For a valid transaction, this will print, 'FINDING_NODE', 'GENERATING_POW', 'SIGNING', 'STORE', 'STORED'
  },
  complete: () => { console.log('Transaction complete') },
  error: error => { console.error('Error submitting transaction', error) }
})
```

## Sending a message

In the following code snippet we send a **Message** from an owned address to the public address `9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM`, by creating a message **Atom** and signing it with our **Identity**. Finally we get the results by subscribing to the transaction updates.

```javascript
const myIdentity = identityManager.generateSimpleIdentity()
const myAccount = myIdentity.account
myAccount.openNodeConnection()
​
// Wait for the account to sync data from the ledger
​
// No need to load data from the ledger for the recipient account
const toAccount = RadixAccount.fromAddress('9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM', true)
​
const message = 'Hello World!'
​
const transactionStatus = RadixTransactionBuilder
  .createRadixMessageAtom(myAccount, toAccount, message)
  .signAndSubmit(myIdentity)
                    
transactionStatus.subscribe({
  next: status => {
    console.log(status) 
    // For a valid transaction, this will print, 'FINDING_NODE', 'GENERATING_POW', 'SIGNING', 'STORE', 'STORED'
  },
  complete: () => { console.log('Transaction complete') },
  error: error => { console.error('Error submitting transaction', error) }
})
```

## Storing an application payload

In the following code snippet we store a **Payload** to the application _my-test-app_ for an owned address and the public address `9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM`, by creating a payload **Atom** and signing it with our **Identity**. Finally we get the results by subscribing to the transaction updates.

```javascript
const myIdentity = identityManager.generateSimpleIdentity()
const myAccount = myIdentity.account
myAccount.openNodeConnection()
​
// Wait for the account to sync data from the ledger
​
// No need to load data from the ledger for the recipient account
const toAccount = RadixAccount.fromAddress('9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM', true)
​
const applicationId = 'my-test-app'
const payload = JSON.stringify({
  message: 'Hello World!',
  otherData: 123
})
​
const transactionStatus = RadixTransactionBuilder
  .createPayloadAtom([myAccount, toAccount], applicationId, payload)
  .signAndSubmit(myIdentity)
                    
transactionStatus.subscribe({
  next: status => {
    console.log(status) 
    // For a valid transaction, this will print, 'FINDING_NODE', 'GENERATING_POW', 'SIGNING', 'STORE', 'STORED'
  },
  complete: () => { console.log('Transaction complete') },
  error: error => { console.error('Error submitting transaction', error) }
})
```

