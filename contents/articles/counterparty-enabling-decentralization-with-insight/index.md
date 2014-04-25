---
title: "Counterparty: Enabling Decentralization with Insight"
author: xnova
date: 2014-04-24
template: article.jade
---

Hello.
My name is Robby Dermody (a.k.a. xnova), and I'm a member of the [Counterparty](https://www.counterparty.co/) Project.
Counterparty is a suite of financial tools in a protocol built on top of the Bitcoin blockchain.
Counterparty provides its users with the world’s first peer-to-peer digital asset exchange, betting functionality, and derivatives market.
We're writing this guest blog post for the bitcore project because of how our team has incorporated [Insight](http://insight.is/) into the Counterparty reference client, for which it has proved invaluable.

When it comes to Counterparty, decentralization, robustness and security are our top priorities.
When we were designing Counterwallet, we didn't want to store any user wallet data on our private servers at all.
All address generation is from a 12-world deterministic wallet passphrase, and all transaction signing is done in the user's browser, so the servers which host Counterwallet need to be able to query the Bitcoin blockchain for the balances and unspent transaction outputs of arbitrary Bitcoin addresses.
Bitcoin Core only knows such information about addresses in its wallet, however.

Indeed, the functionality of Bitcoin Core is in many ways rather limited: its age and idiosyncrasies make it unsuitable for direct use by a lot of great Bitcoin projects, which often resort to using proprietary, closed-source services for getting detailed information about the state of the Bitcoin blockchain that Bitcoin Core cannot provide.
We were ourselves querying Blockchain.info for everything we needed.
But such a service, while very useful, promotes centralization, and cannot have the reliability of a full node implementation of Bitcoin, which Counterparty does require.

Insight provided us with a great alternative, one that allowed us direct access to the Bitcoin blockchain, along with all of the security that such access provides.
Insight also has better support for testnet (and a much better web interface) than any other block explorer.
Bitcoin testnet is an extremely valuable resource which our community does not make nearly enough use of, and the existence of Bitcore should greatly encourage its future use.

Without Insight, Counterparty would have had to release Counterwallet live on mainnet immediately, without being able to bug-test first.
Certainly we found many bugs in Counterwallet while running it on testnet, and an immediate release on mainnet would have made the development and release of Counterwallet harder on both its developers and its users.
Beyond that, Insight's block explorer helps us with debugging.
In addition, we link directly to the public Insight webpages in some places, so that users can, for example, get more information about a block.

Counterparty is live today, and it is running Insight for its backend block explorer.
Achieving our goals would have been much harder, and our software would have been much more fragile, had it not been for the excellence of the bitcore software and the excellence of the entire BitPay team, which has been responsive and helpful whenever possible.

We offer our thanks for all of their work.

&#8212; The Counterparty Dev Team
