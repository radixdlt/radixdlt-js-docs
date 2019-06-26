# Account management

## Introduction

Let's review some code examples on how to manage **Accounts**:

* [Creating a custom account system](https://docs.radixdlt.com/alpha/developer/javascript-client-library-guide/code-examples#creating-a-custom-account-system)

## Creating a custom account system

In the following code snippet we create a custom **Account system**, that enables us to process **Atoms** in any way we might see fit.

```javascript
import { RadixAccountSystem, RadixAtomUpdate, RadixAtom } from 'radixdlt'

export default class CustomAccountSystem implements RadixAccountSystem {
  public name: 'CUSTOM'
  
  public async processAtomUpdate(atomUpdate: RadixAtomUpdate) {
    const atom: RadixAtom = atomUpdate.atom
    
    // This is either 'STORE' or 'DELETE'
    const action = atomUpdate.action
    
    // Do whatever you want with your atom here
    console.log(atom.getAidString())
    if (action === 'STORE') {
      // Add atom contents to state
    } else {
      // Remove atom contents from state
    }
  }
}
```

{% hint style="info" %}
**Note:** the `atomUpdate` has an action field, which can be **STORE** or **DELETE**.

A **DELETE** can occur when an **Atom** fails to validate or is rejected by the consensus. It's crucial to handle it correctly in the particular context of the application.
{% endhint %}

Once you have your custom account system, you can simply add it to the account using the `addAccountSystem()` method:

```javascript
const account = RadixAccount.fromAddress('JEaSfBftmFdseSRKfTm6hJNhQi3FAEqmVzAPTxsf55wPqXvBxRB')
const myAccountSystem = new CustomAccountSystem()

account.addAccountSystem(myAccountSystem)
account.openNodeConnection()
```

