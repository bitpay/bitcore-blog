---
title: "TransactionBuilder: A (hopefully) easy API to generate bitcoin transactions"
author: ematiu 
date: 2014-04-02
template: article.jade
---



# TransactionBuilder

During the last few days we have been working on a new interface to generate
Bitcoin transactions. We wanted to support all standard types transactions 
(pay to pubkeyhash, pay to pubkey, pay to script and multisig)  while providing 
an easy to use API. This is how we arrived to the [Builder Pattern](https://en.wikipedia.org/wiki/Builder_pattern) 
and TransactionBuilder was born.


Hopefully TransactionBuilder will help developers on unleash the power of 
Bitcoin transactions, while maintaining 


## Bitcoin Transaction Introduction
All Bitcoin transactions have inputs and outputs. The inputs are the sources of the coins the transactions is spending (i.e. transfering to the outputs). The sum of all inputs' values  must be equal or greater than the sum of all outputs' values. The difference, if existant, is the fee assigned by the transaction creator to the miner. This fee will be collected by miner that includes the transaction on the Bitcoin blockchain.

Once the transaction is accepted on the network, the outputs are ready to become inputs for future transactions. Unsurprisingly, each output can only be spent once, therefore a common name for the available outputs to became inputs is *unspent outputs*.

In order to spend an *unspent output* in a transaction, proof of ownership must be provided. Bitcoin implement this security mechanisim by adding a script to all outputs (called ScriptPubKey).  This script must be fulfill to spend it. Each time an unpent output is been spended in a transaction input, the input **must** provide the right arguments to the unspent's script (those arguments are a script theirself, and are called ScriptSig).  Failure to do this, will cause the rejection of the transaction by the network. ScriptPubKey and ScriptSig can be thought as a lock and a key, respectivelly.

## Standard Transactions

In Bitcoin standard transactions elliptic curve cryptography is used in ScriptPubKey and ScriptSig. The two more common ScriptPubKey types (*PayToPubKey* and *PayToPubKeyHash*) provides a public key (or its hash) in their contents. In order to spend the output, ScriptSig must provide a signature of the spending transaction using the correspoding private key. The beauty of this mechanisim is that the private key is never revealed. Output's ScriptPubKey has only the public key (or its hash) and Input's ScriptSig has only a signature of the transaction and still the ownership verification can be accomplish by a third party (the bitcoin network in our case).

Besides the two described transaction types, Bitcoin defines *Multisig* and *PayToScriptHash*. Multisig is similar to the described schema, but generalize it by providing multiple (N) public keys on ScriptPubKey and asking for certain number (M) of signatures to allow spending the output. Examples of Multisig configurations are 2 required signatures out of 3 given public keys, 3 out of 5, and 2 out of 2.  Use cases for Multisig include shared control of coins, 2 factor authentication, third party arbitration, etc.

Finally, in PayToScriptHash (or P2SH) ScriptPubKey just contain a hash of a script. In order to spend the transaction, the corresponding script must be provided on the input (on ScriptSig), AND that script must be fullfil with the signatures also included on ScriptSig.

### TransactionBuilder

Without further do, here are some examples:


Transaction generation and signing
``` js
  var bitcore = require('bitcore'); // for browser, check Readme and 
                                    // example/example.html examples.
  var Builder = bitcore.TransactionBuilder;
  var tx = new Builder(opts)
    .setUnspent(utxos)
    .setOutputs(outs)
    .sign(keys)
    .build();

  // Transaction ready for broadcast.
  broadcast(tx.serialize().toString('hex'));
```

In the examples, `utxos` are the available unspent outputs. The `utxos` list for a list of controlled pubkeys can be obtained from bitcoin blockchain APIs, like [blockchain.info](http://blockchain.info), [biteasy.com](http://biteasy.com), [blockr.io](http://blockr.io), or our own open-sourced [insigh-api](https://github.com/bitpay/insight-api). the expected form is:
```
[
    {
      "address": "2Mwswt6Eih28xH8611fexpqKqJCLJMomveK",
      "scriptPubKey": "a91432d272ce8a9b482b363408a0b1dd28123d59c63387",
      "txid": "2ac165fa7a3a2b535d106a0041c7568d03b531e58aeccdd3199d7289ab12cfc1",
      "vout": 1,
      "amount": 1.0,
      "confirmations":7
    }
    , { ... }
]
```

Once the output is defined, `TransactionBuilder` will scan the `unspent outputs` to select the ones needed to fullfill the transaction output amount. Following the reference client implementation, unspent outputs with 6 or more confirmations will be selected first, then outputs with 1 or more confirmation and finally (and only if option `spendUnconfirmed: true` was provided at `opts`) outputs with 0 confirmations. Unless a fixed `fee` is included in `opts`, the fee will be dynamically calculated depending the transaction size, using [bitcoin guidelines](https://en.bitcoin.it/wiki/Transaction_fees).

Once the inputs, outputs and fee are defined, `TransactionBuilder` will add an output to collect the remainder amount, if any, in a remainder out. This out can be provided in `opts` (see examples below), or the first input will be automatically selected.

Other options include: `lockTime` to set transaction's `lock_time`; `signhash` to set the signature types (defaults to *Transaction.SIGHASH_ALL*, other possible values are Transaction.SIGHASH_NONE, Transaction.SIGHASH_ONE and Transaction.SIGHASH_ANYONECANPAY).


It is expected that the set of private `keys` will the match the output's `ScriptPubKey` to unlock them. After `.sign(keys)` is called, `.isFullySigned()` can be call to check if all inputs are properly signed. This schema work with all stardart type of transactions, as demostrated later.

Transaction generation and later signing

``` js
  var b = new Builder(opts)
    .setUnspent(utxos)
    .setOutputs(outs);

  var incompleteTx  = b.build();

  while(!b.isFullySigned()) {
    var keys = getMoreKeys();
    b.sign(keys);
  };
  var completeTx  = b.build();
  broadcast(tx.serialize().toString('hex'));

```

To generate P2SH transactions, the utility `.infoForP2sh` is provided. TODO
generate the

P2SH Transaction generation and later signing

``` js

  var infoForP2sh   = Builder.infoForP2sh({
    nreq    :3, 
    pubkeys:pubkeys, 
    amount  :0.05,
  }, 'testnet');
 
  var outs = [{
    address:infoForP2sh.address, 
    amount:0.05,
  }];

  var map = {};
  map[p2shAddress]=infoForP2sh;

  var info = Builder.  
  var b = new Builder(opts)
    .setUnspent(utxos)
    .setHashToScriptMap(map)
    .setOutputs(outs);

  var incompleteTx  = b.build();

  while(!b.isFullySigned()) {
    var keys = getMoreKeys();
    b.sign(keys);
  };
  var completeTx  = b.build();
  broadcast(tx.serialize().toString('hex'));

```

Sending remainder to a defined output.
``` js
  var opts = {
    remainderOut: {address: 'mwZabyZXg8JzUtFX1pkGygsMJjnuqiNhgd'},
  };
  var tx = new Builder(opts)
    .setUnspent(utxos)
    .setOutputs(outs)
    .sign(keys)
    .build();
```


