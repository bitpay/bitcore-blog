---
title: "Cosign: Our Multisignature Wallet In Development"
author: ryan
date: 2014-03-25
template: article.jade
---

One of the greatest problems in bitcoin is security.
Countless individuals and companies have lost their bitcoins due to compromised private keys.
Multisignature transactions offer a solution to this problem.
They just need to be user-friendly.

That is the idea behind Cosign, a multisignature wallet being developed by the bitcore team.
It should be just as easy to spend multisignature bitcoins as regular ("single signature") bitcoins, except other people (or computers) have to sign the transactions before they are valid.

The way this works is that when someone from a, say, 3-of-5 multisignature wallet wishes to spend bitcoins, they can spend the bitcoins just like normal.
Then the partially signed transaction shows up on their cosigners' screens, which they can choose to sign or not sign.
If three people sign the transaction, then it is broadcast to the bitcoin network.

This provides another layer of protection for theft.
If your private keys are compromised, it doesn't mean your bitcoins are stolen.
The thief has to steal your private keys and those of your cosigners before they can spend the bitcoins.

Though easy to use, Cosign takes every measure to retain the full security of multisignature transactions.
There are a couple of key points that Cosign must satisfy to retain this full security:

1. The private keys must be generated client-side and must remain client-side. If they are ever stored remotely, they must first be encrypted with a strong password.
2. The software must be open-source, and stored and executed client-side, so that it can be audited and cannot be changed by a third party.

