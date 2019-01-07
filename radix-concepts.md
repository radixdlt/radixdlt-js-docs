# Radix concepts

## Introduction <a id="overview"></a>

Let’s dig a bit on the concepts that make Radix a unique distributed ledger technology, so we can share a common language.

## Universe

A **Universe** represents the Radix network. It maintains connections to **Nodes**, and you can ask it to give you a connection to a node that serves a specific **Shard**.

{% hint style="info" %}
Because Radix is built to be sharded from the ground up, it is not enough to have a single connection to the network - depending on what addresses you’re trying to work with, you might need a number of connections.
{% endhint %}

## Shards

A **Shard** is simply a segment of a Universe. A public Radix network \(Universe\) is segmented into a very large shard space \(currently 2^64 shards\). The shard number of an address is deterministically calculated, so it's trivial for anyone to correctly calculate the shard a public key lives on.

## Nodes

A **Node** provides general computing and networking resources to the network. Nodes are responsible for validating events and transactions, relaying messages, resolving conflicts and executing scripts on the network. They also maintain a subset of the shard space, and get fees in proportion to their work.

## Atoms

An **Atom** is the fundamental unit of storage on Radix's distributed ledger. Its structure defines one or more actions which update the ledger's state as an atomic transaction, that is, all-or-nothing.

## Account

An **Account** represents all the data stored for a user on the ledger. This includes tokens, but also arbitrary data, as well as more advanced types of transactions in the future such as multi-sig and Scrypto smart contracts.

## Address

An **Address** lives in a **Shard** and is the start and end point for any **Atom** in the Radix Universe. It's also a reference to an **Account** and allows a user to receive tokens and/or data from other users. A Radix address is generated from a public key and a **Universe** checksum.

{% hint style="info" %}
Keep in mind, the defined **Universe** affects the generated **Address**.
{% endhint %}

## Account Systems

When an **Account** is connected to the network, it will take any incoming **Atoms** and pass them through the account systems. The default systems are:

* **Transfer System**, which keeps a list of transactions involving this account as well as the account balance for all the different tokens in the account
* **Radix Messaging System**, which manages the different Radix messaging chats this account is involved in
* **Data System** used for custom data stored on the ledger

{% hint style="info" %}
You can create your own custom **Account Systems** if you want access to the raw **Atoms**.
{% endhint %}

## Identity

An **Identity** represents a private key which can sign **Atoms** and read encrypted data. This private key can be stored in the application, or in the future, it might live elsewhere such as the users wallet application or hardware wallet.

{% hint style="info" %}
The only type of **Identity** currently available is the _**Simple Identity,**_ and it has a private key stored in memory.
{% endhint %}

## Transaction Builder

The **Transaction Builder** handles creating and submitting to the network any kind of **Atoms** that the Radix ledger can accept. Right now this means _token transfer_ atoms, _data payload_ atoms and _Radix messaging_ atoms \(which are just a special case of the data payload atoms\). In the future the atom model will be a lot more powerful.

## Faucet Service

The **Faucet** service is a simple development service running on the Radix network that sends free test tokens back to any account that sends a message to it.

{% hint style="info" %}
In the **ALPHANET** Universe, the **Faucet** service address is `9ey8A461d9hLUVXh7CgbYhfmqFzjzSBKHvPC8SMjccRDbkTs2aM`
{% endhint %}

