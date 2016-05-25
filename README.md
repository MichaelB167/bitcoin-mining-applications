[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# Bitcoin Mining Applications

Everyone has heard of bitcoin and some of you have probably made transactions using it, maybe as a payment method or a speculative investment.

About a year ago, I was talking with a friend and trying to explain why I felt confident having myself invested in it.  I understood the basic principles of how it operated as a digital currency, but as I tried explaining even the basics of encryption and the mining process all that was coming out was the bitcoin equivalent of Ruby Magic.

This talk will compile the bits and pieces I've learned since then to answer the above, as well as go into a bit more detail on how bitcoin relates to subjects we've covered in class such as cs data structures and network funcionality.

## Objectives

By the end of this, developers should:

-   Understand the basic principles of bitcoin.
-   See some parallels between bitcoin mining networks and cs data structures that we've seen in class
-   Know where to find starter code for integrating bitcoin payments into projects
-   Be able to mine sample bitcoins via the command line in a sandbox p2p environment.
-   Hopefully not be on an NSA watchlist or have inadvertently committed any cyber crimes.

## Bitcoin Cliffs

Bitcoin was invented by a person with the pseudonym Satoshi Nakamoto as a way to mitigate fraud in online transactions.  They postulated that the current system of relying on financial institutions as third parties to process transactions is risky because it introduces a significant need for trust between parties.  Bitcoin instead allows two parties to interact directly with each other based on cryptographic proof and records transactions in a public ledger called the 'block chain' which we'll cover momentarily.

Their vision has only been partially achieved.  Bitcoin's proponents will note that its value has increased from pennies in 2012 to ~450 USD today and currently averages ~250k transactions/day in the largest Bitcoin wallet, Blockchain.

However, the same fasciliation of near total anonymity and authentication standards that made Bitcoin successful have made it a target for crime.  The most well-known cases are probably its fascilitation of a wide variety of illegal transactions on Silk Road and the disappearance of hundreds of millions of dollars worth of Bitcoin from the servers of an exchange called Mt Gox run by this super trustworthy looking guy:

<img src="http://i.imgur.com/DIWlCuJ.png?1">

In short, Bitcoin has a short but wildly varied history.  Further exploring the merits of bitcoin and predicting its future are fascinating but lengthy discussions.  For our purposes, let's go under the hood to explore what a bitcoin actually is and check out the software mechanisms that produce them.

## Bitcoin components

Have a look at the running transaction log on [blockchain.info](https://github.com/ga-wdi-boston/meta/wiki/ForkAndClone) and click on a transaction (this is just a popular wallet, not to be confused with the block chain data structure).  Perhaps surprisingly, you can not only see all the transactions occuring but you can see details for each one including wallet addresses and hash keys such as these which originated from Barcelona:

address `1Ath6yhq2uoMUMXTpBot2rNRsdRYS39L2m`

hash `6c7fd28c807427b387d5813702aa901ee7319bce`

Bitcoin wallet services provide users with a free alphanumeric address that, like an ip address, gives them a unique location on the Bitcoin network.  The above hash is just the public portion of a randomly generated token similar to those we've used in generating user credentials for our projects.  If you like, you can create your own and send/receive BTC using [this sandbox wallet](https://sandbox.coinbase.com/).

To complete a transaction, the payer of the Bitcoin being transferred must provide their own private key to ensure validation:

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/ce/Bitcoin_Transaction_Visual.svg/361px-Bitcoin_Transaction_Visual.svg.png">

At it's essence, bitcoin is a digital file that is structured with key value pairs (names and amounts) that are updated when a transaction occurs.

However, such a transaction isn't local to one user or machine.  We can see all transaction addresses/hashes because 'Power users' who provide bitcoin mining services all maintain a copy of this file, and they are all updated accordingly when BTC changes holders.  Who are these power users?  To answer that, we need to delve into Bitcoin nodes and the block chain.

## Bitcoin Nodes

Nodes in this context are simply a network of machines running bitcoin software (such as bitcoin core which we will use to create a test network) that broadcast transaction updates to other nodes.  To help visualize that process, check out this gif of a node connecting to the bitcoin network in real time (the origin node is the first green sphere):

<img src="https://j.gifs.com/pYPmAr.gif">

Each individual node records transactions, adds them to the machine's local record, and then broadcasts any changes to peer nodes across the network.  These changes are then published in a public database, which brings us to...

## What is the block chain?

The block chain is a public ledger/database which is operated on by the aforementioned nodes.  Remember when you almost had your university credentials rescinded for torrenting Wings of Liberty without a vpn on the school network?  Bitcoin works in a similar capacity, albeit by providing tangible financial rewards for providing peer-to-peer mining services.

In contrast to the tree structure we've seen in class, the block chain relies on a structure called the merkle/hash tree which recursively hashes nodes together until they are consolidated into one.

<img src="http://i.imgur.com/GCvX5Jh.png?1  ">

Miners/power users operate server banks which perform this operation repeatedly to consolidate transactions and verify their authenticity.  Again, this is where the peer-to-peer nature of the Bitcoin network comes in.

To be accepted by the network, blocks must contain an arbitray number (nonce) that is determined by a brute force algorithm even more advanced than my tic tac toe solution.  Thanks to testing environments, we can replicate this system on a smaller scale to see what kind of operations real Bitcoin farming software carries out on a constant basis.

## Lab: Create our own (test) Bitcoin mining consortium

1.  Follow along to configure bitcoind
1.  Run `bitcoind -regtest -daemon` which should yield 'bitcoin server starting'
1.  Set bitcoind to testing mode with `bitcoin-cli -regtest setgenerate true 101`
1.  Verify balance with `bitcoin-cli -regtest getbalance`- you should have 50 bitcoin available.

## Project Integration

If you want to enable bitcoin payments in your web app, stripe provides useful tools for doing so.  First, create a checkout handler/event such as-
```
  let handler = StripeCheckout.configure({
    key: 'pk_test_La0sDwdMY4vZwGyewISFrpm4',
    image: '/img.png',
    locale: 'auto',
    token: function(token) {
    }
  });

  $('#checkoutBtn').on('click', function(e) {
    handler.open({
      name: 'Site name',
      description: 'Buy Stuff',
      amount:
      bitcoin: true
    });
    e.preventDefault();
  });
  ```

You must also create a reciever object that can accept bitcoin payments when an order is created.  An example post request to do so would be:
```
curl https://api.stripe.com/v1/bitcoin/receivers \
   -u sk_test_uDTLkYo56xbc6pDAl6Y3oPZT: \
   -d amount=100 \
   -d currency=usd \
   -d description="Receiver for John Doe" \
   -d email="test@example.com"
```
Yielding a massive json string, the highlights of which are:

```
"bitcoin_amount": 1757908,
  "bitcoin_amount_received": 0,
  "bitcoin_uri": "bitcoin:test_7i9Fo4b5wXcUAuoVBFrc7nc9HDxD1?amount=0.01757908",
  ```

  Amount being the number of bitcoins that the user must send denominated in 10^8 BTC.  Additional documentation can be found [here](https://stripe.com/docs/api#bitcoin_receiver_object)

## Additional Resources

-   Original bitcoin whitepaper- https://bitcoin.org/bitcoin.pdf
-   Stripe docs for bitcoin payment integration https://stripe.com/docs/bitcoin
-   Test wallet https://sandbox.coinbase.com/
-   Create a test mining environment on your machine https://github.com/bitcoin/bitcoin (beware, this download is enormous)
-   Merkle trees http://chimera.labs.oreilly.com/books/1234000001802/ch07.html#merkle_trees


## [License](LICENSE)

Source code distributed under the MIT license. Text and other assets copyright
General Assembly, Inc., all rights reserved.
