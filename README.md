[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# Bitcoin Mining Applications

Everyone has heard of bitcoin and some of you have probably made transactions using it, maybe as a payment method or a speculative investment.

About a year ago, I was talking with a friend and trying to explain why I felt confident having myself invested in it.  I understood the basic principles of how it operated as a digital currency, but as I tried explaining even the basics of encryption and the mining process all that was coming out was the bitcoin equivalent of 'webscale brah'.

This talk will succinctly compile the bits and pieces I've learned since then to answer the above, as well as go into a bit more detail on how bitcoin relates to subjects we've covered in class such as cs data structures and network funcionality.

## Objectives

By the end of this, developers should:

-   Understand the basic principles of cryptocurrencies and more specifically bitcoin.
-   See parallels between bitcoin mining networks and cs data structures that we've seen in class
-   Be able to mine sample bitcoins via the command line in a sandbox p2p environment.
-   Hopefully not be on an NSA watchlist or have inadvertently committed any cyber crimes.

## Bitcoin Cliffs

Bitcoin was invented by a person with the pseudonym Satoshi Nakamoto as a way to mitigate fraud in online transactions.  They postulated that the current system of relying on financial institutions as third parties to process transactions is risky because it introduces a significant need for trust between parties.  Bitcoin instead allows two parties to interact directly with each other based on cryptographic proof and records transactions in a public ledger called the 'block chain' which we'll cover momentarily.

Their vision has only been partially achieved.  Bitcoin's proponents will note that its value has increased from pennies in 2012 to ~450 USD today and currently averages ~250k transactions/day in the largest Bitcoin wallet, Blockchain.

However, the same fasciliation of near total anonymity and authentication standards that made Bitcoin successful have made it a target for crime.  The most well-known cases are probably its fascilitation of a wide variety of illegal transactions on Silk Road and the disappearance of hundreds of millions of dollars worth of Bitcoin from the servers of an exchange called Mt Gox run by this super trustworthy looking guy:

<img src="http://i.imgur.com/DIWlCuJ.png?1">

In short, Bitcoin has a short but wildly varied history.  Further exploring the merits of bitcoin and predicting its future are fascinating but lengthy discussions.  For our purposes, let's go under the hood to explore what a bitcoin actually is and check out the software mechanisms that produce them.

## Bitcoin components

Have a look at the running transaction log on [blockchain.info](https://github.com/ga-wdi-boston/meta/wiki/ForkAndClone) and click on a transaction (this is just a popular wallet, not to be confused with the blockchain data structure).  Perhaps surprisingly, you can not only see all the transactions occuring but you can see details for each one including wallet addresses and hash keys such as these which originiated from Barcelona:

address `1Ath6yhq2uoMUMXTpBot2rNRsdRYS39L2m`

hash `6c7fd28c807427b387d5813702aa901ee7319bce`

Bitcoin wallet services provide users with a free alphanumeric address that, like an ip address, gives them a unique location on the Bitcoin network.  The above hash is just the public portion of a randomly generated token similar to those we've used in generating user credentials for our projects.  If you like, you can create your own and send/receive BTC using [this sandbox wallet](https://sandbox.coinbase.com/)

At it's essence, bitcoin is a digital file that is structured with key value pairs (names and amounts) that are updated when a transaction occurs.

However, such a transaction isn't local to one user or machine.  We can see all transaction addresses/hashes because 'Power users' who provide bitcoin mining services all maintain a copy of this file, and they are all updated accordingly when BTC changes holders.  Who are these power users?  To answer that, we need to delve into Bitcoin nodes and the Blockchain.

## Bitcoin Nodes

Nodes in this context are simply a network of machines running bitcoin software that broadcast transaction updates to other nodes .  To help visualize that process, check out this gif of a node connecting to the bitcoin network in real time (the origin node is the first green sphere):

<img src="https://j.gifs.com/pYPmAr.gif">



## Demo: Write a Demo



## Exercise: Write an Exercise



## Lab: Write a Lab

## TL;DR

## Additional Resources

-   Any useful links should be included in the talk material where the link is
    first referenced.
-   Additional links for further study or exploration are appropriate in this
    section.
-   Links to important parts of documentation not covered during the talk, or
    tools tangentially used but not part of the focus of the talk, are also
    appropriate.


## [License](LICENSE)

Source code distributed under the MIT license. Text and other assets copyright
General Assembly, Inc., all rights reserved.
