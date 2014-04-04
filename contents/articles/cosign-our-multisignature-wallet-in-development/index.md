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

That is the idea behind Cosign, a multisignature wallet being developed by the Bitcore team.
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

Cosign takes advantage of a number of modern browser and bitcoin technologies to make this possible.
Web RTC is used to establish p2p connections between cosigners.
HTML5 local storage is used to store the wallet.
HD extended keys are used to simplify the generation of new addresses.

It works like this.
Five people (or thereabouts) wish to participate in multisignature transactions where three of them (or so) are required to sign transactions.

First, one of them opens up Cosign and creates a new wallet, and then shares the ID of the wallet with the four cosigners.
All of the cosigners join the wallet and generate a new extended private key, which has a corresponding extended public key.
The extended public key is shared with the others.
The extended private key is kept private.

Now the cosigners can view their multisignature wallet just like a normal wallet.
The appearance and workflow of the wallet are almost exactly the same, with only one catch:
When someone wishes to send bitcoins, the bitcoins are not immediately sent.
Instead, the partially signed transaction is shared with the other cosigners.
If three of them sign it, then the transaction is complete, and can be broadcast to the bitcoin network and stored in the blockchain.

## Mockups

Thanks to Bechi, we now have some beautiful mockups of this workflow.

![1](/blog/images/cosign-1-join.jpg)

First, one of the cosigners opens Cosign and creates a new wallet.
The wallet ID is a random number that is automatically generated.
This ID is then shared with the other cosigners so they can all visit the wallet.
The other cosigners copy+paste the ID to join the wallet, similar to sharing a Google Hangouts link.

![2](/blog/images/cosign-2-home.jpg)

Upon joining a wallet, the cosigners will generate a new, random master extended private key, which will be used to generate master public keys, which are automatically shared with the other members of the wallet.
Addresses formed from these master keys can be seen in the addresses list, just like a normal bitcoin wallet.

![3](/blog/images/cosign-3-txs.jpg)

The transactions page is where the magic happens.
This page looks almost the same as a regular transactions page, with one key difference: When a cosigner sends bitcoins, it is regarded as "partially signed," and shows up on the screens of the other cosigners.
The other cosigners can either choose to sign or to ignore the transaction.
If enough cosigners sign the transaction (say, three, in the case of a 3-of-5 multisig wallet), the transaction is fully signed and automatically broadcast to the bitcoin network.

![4](/blog/images/cosign-4-send.jpg)

When a cosigner wishes to send bitcoins, they visit the send page, just like a normal bitcoin wallet.
The only difference is that the transaction is not broadcast to the bitcoin network until sufficiently many cosigners sign the transaction.

![5](/blog/images/cosign-5-backup.jpg)

The wallet, which contains only the public keys of the cosigners, should be backed up in case of disaster.
So long as the wallet is backed up and at least (say) three of the cosigners have their master private keys, the bitcoins can be recovered.

It's worth discussing some of the technologies we are using to build this projects and why.

## HD Wallets

If you've ever considered how best to handle multisignature transactions, you've encountered the difficulty in sharing public keys with the cosigners.
Every time you generate a new, random private key, you have to share the corresponding public key, which is burdensome.
Either that, or you re-use the same addresses over and over again, which is terrible for privacy.

HD wallets as described in [BIP 32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) solve this problem by allowing one master extended private key to generate, deterministically, infinitely many private/public key pairs.
Importantly, extended public keys can be derived from the master extended public key.
This means you share one extended public key with your cosigners, and there is no need to share more public keys after that.
All of the cosigners have every address that will ever be generated right from the start.
This greatly increases the ease of sharing public keys.

## Web RTC

Cosign is a web application that is run locally.
By using web technologies, we're able to make an app that can run pretty much anywhere, and can be improved and audited by a huge number of developers.
One of the neatest new pieces of technology to come to the web is [Web RTC](http://www.webrtc.org/), which allows (almost) p2p connections in the browser.
The only central element of this is a server that exists solely to facilitate the creation of the p2p connection, after which the central server is no longer needed.

The ultimate goal of Cosign is to have a genuinely p2p app with no need whatsoever for a central server, but that is not yet possible from the browser, so we will get as close to that as we can.

## HTML 5 Local Storage

Running apps in the browser used to be far more limited, but today, HTML 5 local storage allows apps to keep significant amounts of data stored locally so that no central server is required to store anything.
The public wallet (containing the public keys) is shared amongst all cosigners, as are other critical pieces of information such as unspent transaction outputs.

Between Web RTC and HTML 5 local storage, it is actually possible to have a web wallet that runs locally with no need for a central server except the p2p facilitator. Oh, and one more important piece.

## Insight and SPV

Cosign is developed by the same people that developed <a href="http://insight.is/">Insight</a>, a powerful, open-source blockchain API and client.
Cosign uses insight to find unspent transaction outputs and broadcast transactions to the bitcoin network.
Long-term, however, we believe it will be possible to implement [SPV](https://en.bitcoin.it/wiki/Scalability#Simplified_payment_verification) in the browser, where the only need for a server will be to bridge the Web RTC communication protocol with the real bitcoin p2p protocol.
In the meantime, running insight locally means there is no central server except the p2p facilitator.

## Conclusion

Cosign development is only just now getting underway.
Look forward to more discussion of the technical challenges along the way as we build the most secure wallet in the world.


