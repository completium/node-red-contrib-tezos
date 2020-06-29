<center> <img src="nodes/icons/tezos-logo.png" alt="Tezos logo"  height="100" style="padding-top:50px"> </center>

# node-red nodes for Tezos
[Tezos](https://tezos.com/) is an open-source blockchain for assets and applications that can evolve by upgrading itself. Stakeholders govern upgrades to the core protocol, including upgrades to the amendment process itself.

This package provides the following node-red nodes to interact with the Tezos blockchain:
* transfer
* originate
* call-contract
* generate-keys
* get-contrat
* confirm

> The RPC provider is set in the configuration panel of the nodes.

## transfer
Transfers Tezis from an account to another.

| Parameter        | Desc           | Msg.payload entry  |
| ------------- |:-------------:| -----:|
| RPC provider      | RPC provider URL | - |
| Secret | Private key of sender   |    secret |
| Faucet      | Faucet in JSON format  of sender (if no private key)|   faucet |
| Destination | Address of receiver | destination |
| Amount | Amount of Tezis to transfer | amount |

This node may be paramterized by the following payload JSON object:
```
{
    "secret": "... sender's private key ...",
    "destination": "tz1... receiver address...",
    "amount": ... number of tezis ...
}
```
If success, the node returns the following payload:
```
{
    "res": true,
    "op": { ... operation object ... }
}
```
If fail:
```
{
    "res": false
}
```


## originate
Originates a smart contract.

| Parameter        | Desc           | Msg.payload entry  |
| ------------- |:-------------:| -----:|
| RPC provider      | RPC provider URL | - |
| Secret | Private key of originator  |    secret |
| Faucet      | Faucet in JSON format  of originator  (if no private key)|   faucet |
| Code | Michelin codes for code in JSON | code |
| Storage | Michelin codes for storage in JSON | storage |

This node may be paramterized by the following payload JSON object:
```
{
    "secret": "... originator's private key ...",
    "code": [ ... Michelin code of contract ... ],
    "storage": { ... Michelin code of contract  storage ... }
}
```

The Michelin code may be obtained from Tezos client's originate command in dry and verbose mode.

If success, the node returns the following payload:
```
{
    "res": true,
    "op": { ... operation object ... }
}
```
If fail:
```
{
    "res": false
}
```

## call-contract
Calls a smart contract's entry point.

| Parameter        | Desc           | Msg.payload entry  |
| ------------- |:-------------:| -----:|
| RPC provider      | RPC provider URL | - |
| Secret | Private key of originator  |    secret |
| Faucet      | Faucet in JSON format  of originator  (if no private key)|   faucet |
| Address | Address of the smart contract | addr |
| Entry | Name of the entry point to call | entry |
| Arguments | List of arguments to pass | args |
| Amount | Amount of tezis to transfer to the call (0 if not specified)| amount |

This node may be paramterized by the following payload JSON object:
```
{
    "secret": "... originator's private key ...",
    "addr": "KT1... address of the smart contract ... ",
    "entry": "... name of the entry point ...",
    "args": [ ... list of arguments ... ],
    "amount" : ... number of tezis to transfer ...
}
```

For example, say the contract has an `exec` entry point which takes a string and an integer as argments; the following is an example value of the `args` entry:

```
{
    "args" : [ "this is a string", 1 ]
}
```

> The order in which arguments are passed may be derived for example from the "interact" panel of [you.better-call.dev](https://you.better-call.dev/)

If success, the node returns the following payload:
```
{
    "res": true,
    "op": { ... operation object ... }
}
```
If fail:
```
{
    "res": false
}
```

## generate-keys
Generates keys for a new account.

| Parameter        | Desc           | Msg.payload entry  |
| ------------- |:-------------:| -----:|
| Mnemonic      | List of mnemonic words (optional) | mnemonic |

If no menomic is provided, the node generates a random one.

It generates the following payload structure:
```
{
    "mnemonic" : [... list of mnemonic words ...],
    "publicKeyHash": "...",
    "publicKey": "...",
    "privateKey": "..."
}
```


## get-contract
Set msg.payload to be the POJO version of the storage of a smart contract.

| Parameter        | Desc           | Msg.payload entry  |
| ------------- |:-------------:| -----:|
| RPC provider      | RPC provider URL | - |
| Address | Address of the smart contract | addr |

> This node does not currently work for big maps.

## confirm
Confirms an operation.

| Parameter        | Desc           | Msg.payload entry  |
| ------------- |:-------------:| -----:|
| Confirmation | Number of confirmations to wait for | - |
| Operation | operation object | op |

> Operations object are currently too big for node-red to be passed from one node to the other ...

***

All nodes except `generate-keys` use the [Taquito](https://tezostaquito.io/) library.

<img src="https://tezostaquito.io/img/Taquito.png" height="40">


The `generate-key` node uses the [Conseiljs](https://cryptonomic.github.io/ConseilJS/#/) library.

***

Nodes developped by

<img src="nodes/icons/Chaintelligence-logo-black.png" height="80">

a service by [edukera](www.edukera.com).

