---
title: "Bitcore now has 100% Bitcoin Core test data coverage"
author: manu
date: 2014-04-11
template: article.jade
---


We've achieved an important milestone in bitcore development, 
and one that we've been working on since we started with this open source initiative. 
That is achieving **100% compatibility with test data from Bitcoin
Core**. It was hard work, but we think it's worth it: we are now sure 
bitcore handles correctly all the edge cases and tricky input data
the same way that bitcoind does (or at least those that have test data for them). 

The current version of bitcore has **2868** tests passing both in node
and in modern browsers (save for a few tests that make no sense for
the browser yet, like connecting to the p2p bitcoin network). We are
also looking to expand the test coverage, and welcome any contributions
to this important task. If you've thought about collaborating with 
bitcore but didn't know where to start, writing tests is a good way
to internalize with the source code, learn about bitcoin, and improve
the project a lot. You can also follow <a href="/blog/articles/how-to-contribute/">our collaboration guide</a> if
you are new to open-source contributions.

While this milestone is important (test data compatibility with Bitcoin 
Core is a must for any serious bitcoin library), we're still looking
to complete a more ambitious goal: achieving 100% code coverage 
for all our code. A good tool to measure that is <a href="http://gotwarlost.github.io/istanbul/">
istanbul</a>. Here's bitcore's istanbul report:

<img src="/blog/images/bitcore-istanbul.png"> </img>

As you can see, there's some work to be done, but we hope you'll help us
get there! Bitcore is a community effort and adding tests will greatly improve
the quality of the code.








