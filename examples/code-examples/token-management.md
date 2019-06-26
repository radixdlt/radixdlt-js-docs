# Token management

[![License MIT](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/radixdlt/radixdlt-js/blob/master/LICENSE)
[![Build Status](https://travis-ci.com/radixdlt/radixdlt-js.svg?branch=develop)](https://travis-ci.com/radixdlt/radixdlt-js)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=radixdlt-js&metric=alert_status)](https://sonarcloud.io/dashboard?id=radixdlt-js) 
[![Reliability](https://sonarcloud.io/api/project_badges/measure?project=radixdlt-js&metric=reliability_rating)](https://sonarcloud.io/component_measures?id=radixdlt-js&metric=reliability_rating)
[![Security](https://sonarcloud.io/api/project_badges/measure?project=radixdlt-js&metric=security_rating)](https://sonarcloud.io/component_measures?id=radixdlt-js&metric=security_rating) 
[![Code Corevage](https://sonarcloud.io/api/project_badges/measure?project=radixdlt-js&metric=coverage)](https://sonarcloud.io/component_measures?id=radixdlt-js&metric=Coverage)

## Introduction

Let's review the following code examples on how to store and load private keys:

* Creating tokens
* Minting tokens
* Burning tokens

## Links

| Link | Description |
| :----- | :------ |
[radixdlt.com](https://radixdlt.com/) | Radix DLT Homepage
[documentation](https://docs.radixdlt.com/) | Radix Knowledge Base
[forum](https://forum.radixdlt.com/) | Radix Technical Forum
[@radixdlt](https://twitter.com/radixdlt) | Follow Radix DLT on Twitter

## Creating tokens

Tokens can be single or multi-issuance. Multi-issuance tokens can be minted after token creation, while single issuance tokens are limited to the amount specified in the token definition.

A token is uniquely identified by its symbol and the account that owns it. It can be referred to with a unique URI in the form of `/address/symbol`. We call these URIs Radix Resource Identifiers or RRIs for short.

Token amounts on the ledger are stored as integer values in subunits. All tokens have 10^18 subunits.

Granularity defines the minimum increment of tokens that can be transacted, in subunits. For example, if we set granularity to 10^18, then 1 or 2 are valid token amounts, but, 1.5 is not.

```javascript
const symbol = 'EXMP'
const name = 'Example Coin'
const description = 'My example coin'
const granularity = new BN(1) // In subunits
const amount = 1000 // In token units
const iconUrl = 'http://a.b.com/icon.png'

new RadixTransactionBuilder().createTokenSingleIssuance(
  myIdentity.account,
  name,
  symbol,
  description,
  granularity,
  amount,
  iconUrl,
).signAndSubmit(myIdentity)
.subscribe({
  next: status => {console.log(status)},
  complete: () => { console.log('Token defintion has been created') },
  error: error => { console.error('Error submitting transaction', error) }
})

```

After that you can observe the token in the accounts `tokenDefinitionSystem`

```javascript
const tokenReference = `/${myIdentity.account.getAddress()}/${symbol}`


myIdentity.account.transferSystem.tokenUnitsBalance[RLAU_URI] // will be equal to amount
myIdentity.account.tokenDefinitionSystem.getTokenDefinition(symbol) // will return token defintion
```

### Querying the ledger for token definitons
It is possible to query the ledger for token information such as the total supply without manually managing the account using the `radixTokenManager`

```javascript
import {radixTokenManager} from 'radixdlt'

const tokenReference = `/${myIdentity.account.getAddress()}/${symbol}`
radixTokenManager.getTokenDefinition(tokenReference).then(tokenDefinition => {
  tokenDefinition.totalSupply // Check the total supply
}).catch(error => {
  // Token wasn't found for some reason
})
```

## Minting tokens

For multi-issuance tokens, it is possible to mint more tokens after they have been created

```javascript
const tokenReference = `/${myIdentity.account.getAddress()}/${symbol}`
const amount = 10

RadixTransactionBuilder.createMintAtom(myIdentity.account, tokenReference, amount)
.signAndSubmit(myIdentity)
.subscribe({
  next: status => {console.log(status)},
  complete: () => { console.log('Tokens have been minted') },
  error: error => { console.error('Error submitting transaction', error) }
})

```

## Burning tokens

For multi-issuance tokens, it is possible to burn tokens after they have been created
The account that owns the token definiton must also hold the tokens to be burned

```javascript
const tokenReference = `/${myIdentity.account.getAddress()}/${symbol}`
const amount = 10

RadixTransactionBuilder.createBurnAtom(myIdentity.account, tokenReference, amount)
.signAndSubmit(myIdentity)
.subscribe({
  next: status => {console.log(status)},
  complete: () => { console.log('Tokens have been burned') },
  error: error => { console.error('Error submitting transaction', error) }
})
```




