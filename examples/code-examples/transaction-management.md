# Transaction management

## Introduction

Let's review a few code examples related to `RadixTransactionBuilder`, and see how we can handle transactions, messages and payloads:

* [Sending a transaction](transaction-management.md#sending-a-transaction)
* [Sending a message](transaction-management.md#sending-a-message)
* [Storing an application payload](transaction-management.md#storing-an-application-payload)

## Sending a transaction

In the following code snippet we send a transaction from an owned address to the public address `JEaSfBftmFdseSRKfTm6hJNhQi3FAEqmVzAPTxsf55wPqXvBxRB`, by creating a transfer **Atom** and signing it with our **Identity**. Finally we get the results by subscribing to the transaction updates.

```javascript
const myIdentity = identityManager.generateSimpleIdentity()
const myAccount = myIdentity.account
myAccount.openNodeConnection()

// Wait for the account to sync data from the ledger

// No need to load data from the ledger for the recipient account
const toAccount = RadixAccount.fromAddress('JEaSfBftmFdseSRKfTm6hJNhQi3FAEqmVzAPTxsf55wPqXvBxRB', true)
const token = radixUniverse.nativeToken // The default platform token
const amount = 123.12

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

In the following code snippet we send a **Message** from an owned address to the public address `JEaSfBftmFdseSRKfTm6hJNhQi3FAEqmVzAPTxsf55wPqXvBxRB`, by creating a message **Atom** and signing it with our **Identity**. Finally we get the results by subscribing to the transaction updates.

```javascript
const myIdentity = identityManager.generateSimpleIdentity()
const myAccount = myIdentity.account
myAccount.openNodeConnection()

// Wait for the account to sync data from the ledger

// No need to load data from the ledger for the recipient account
const toAccount = RadixAccount.fromAddress('JEaSfBftmFdseSRKfTm6hJNhQi3FAEqmVzAPTxsf55wPqXvBxRB', true)

const message = 'Hello World!'

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

In the following code snippet we store a **Payload** to the application _my-test-app_ for an owned address and the public address `JEaSfBftmFdseSRKfTm6hJNhQi3FAEqmVzAPTxsf55wPqXvBxRB`, by creating a payload **Atom** and signing it with our **Identity**. Finally we get the results by subscribing to the transaction updates.

```javascript
const myIdentity = identityManager.generateSimpleIdentity()
const myAccount = myIdentity.account
myAccount.openNodeConnection()

// Wait for the account to sync data from the ledger

// No need to load data from the ledger for the recipient account
const toAccount = RadixAccount.fromAddress('JEaSfBftmFdseSRKfTm6hJNhQi3FAEqmVzAPTxsf55wPqXvBxRB', true)

const applicationId = 'my-test-app'
const payload = JSON.stringify({
  message: 'Hello World!',
  otherData: 123
})

const transactionStatus = RadixTransactionBuilder
  .createPayloadAtom(myAccount, [toAccount], applicationId, payload)
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

