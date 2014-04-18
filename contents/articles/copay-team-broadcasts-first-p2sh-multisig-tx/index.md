---
title: "Copay Team Broadcasts First BIP32 P2SH Multisig Transaction from Tucuman, Argentina"
author: ryan
date: 2014-04-18
template: article.jade
---

It's really hard to properly secure bitcoins.
It's so hard that even the most technically adept bitcoiners sometimes get hacked and lose their bitcoins.
As bitcoin grows up to become an adult financial system, it is absolutely imperative that we end this security nightmare as quickly as possible.
That is what multisignature transactions promise to do.

The basic security problem with bitcoin is that your private keys give access to your bitcoins.
If you slip up just once and reveal your private keys, which is really easy to do (especially if you&#8217;re not trained in the art of computer security), your bitcoins can be stolen.
Many individuals and companies have lost some or all of their bitcoins this way.

Multisignature transactions offer a solution to this problem by allowing for more than one private key to control an address.
In a 2-of-2 multisignature transaction, a private key from your laptop is used to sign a transaction, and a private key from your phone must also be used to sign the same transaction.
If you slip up and reveal the private keys on your laptop, your bitcoins are still safe because the private keys on your phone haven't been compromised.

Multisignature transactions allow, in principle, any combination of M signatures for N possible public keys.
(Note that limits in bitcoin's architecture don't allow very large M and N to be used.)
For instance, a company might desire to use 3-of-5 multisignature transactions to store their corporate funds.
This is where at least three people of five must sign a transaction.
If one person trips up and leaks their private keys, the company's corporate funds are still safe.

A few weeks ago we announced a new project that we are developing called [Copay](/blog/articles/cosign-our-multisignature-wallet-in-development/) (formerly Cosign) that should make it really easy to do multisignature transactions.
Since then, all six Copay developers have congregated in Tucuman, Argentina to build a prototype of this wallet.

Today we are happy to announce that we have successfully broadcast the first BIP32 P2SH multisig transaction using Copay.
[It can be viewed on Insight here.](http://test.insight.is/tx/22ca516de52794efaec60a1dba3048152b49a3764d2e99124893ac0862cd7928)
(Technical jargon "BIP32" and "P2SH" will be explained in future blog posts.)

Copay is not the only multisig wallet in development.
We fully support all efforts to improve the security of bitcoin, including [BitGo](https://www.bitgo.com/), [CryptoCorp](https://cryptocorp.co/), and [GreenAddress.it](https://greenaddress.it/en/).
We want the best security to win, which will be better for the entire bitcoin ecosystem.
Therefore, in keeping with the spirit of bitcoin, [Copay is 100% open-source](https://github.com/bitpay/copay), and has been since Day 1.
If our ideas are good for security, then they will be adopted by all bitcoin wallets.
We will all win.

We believe 2014 will go down in bitcoin history as the year of multisig.
May stolen bitcoins be a thing of the past.

![Copay Team](/blog/images/copay-first-transaction.jpg)

[@ryanxcharles](https://twitter.com/ryanxcharles), [@maraoz](https://twitter.com/maraoz), [@bechilandia](https://twitter.com/bechilandia/), [@cmgustavo83](https://twitter.com/cmgustavo83), [@colkito](https://twitter.com/colkito), [@ematiu](https://twitter.com/ematiu)

![BitPay Curly](/blog/images/copay-bitpay-yurii.jpg)

