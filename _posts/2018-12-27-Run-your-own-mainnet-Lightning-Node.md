---
  layout: post
  date: 2018-01-20 12:00:00 -0500
  title: "Run your own mainnet Lightning Node"
  subtitle: "Go from nothing to stickers using the instant Lightning network"
  tags: [Bitcoin, Lightning Network, Blockchain, Blockstream, P2p Payments]
  author: Doug von Kohorn
  author_handle: dougvk
  categories:
  published: false
  image: "assets/images/posts/Run-your-own-mainnet-Lightning-Node/other-node-square.png"
  excerpt: "With Lightning’s payment channels, Bitcoin has a chance to prove it
  can scale. I eagerly anticipated the release of the v1.0 Lightning
  specification. I wasn’t motivated enough to run it, however, until Blockstream
  launched its online store that only accepts Lightning payments. Stickers?
  Count me in."
---

With Lightning’s payment channels, Bitcoin has a chance to prove it can scale. I
eagerly anticipated the release of the v1.0 Lightning specification. I wasn’t
motivated enough to run it, however, until Blockstream launched its [online
store that only accepts Lightning payments](https://store.blockstream.com/).
Stickers? Count me in.

{% include image.html
url="assets/images/posts/Run-your-own-mainnet-Lightning-Node/my-other-node-is-a-satellite.png"
description="The omphalos of my universe." %}

The goal of this tutorial is to get you from no bitcoin node and no lightning
node to running fully validating nodes — in under 24 hours. The majority of
hours come from syncing the Bitcoin blockchain, so **I recommend starting this
process before you go to bed and finishing it when you wake up.**

This tutorial **assumes you know how to spin up a Digital Ocean box (or
something similar) running Docker.** You won’t need to understand the
internals — just copy and paste commands — but I encourage you to read up on all
of this. I’ll put an appendix of relevant resources at the end.

This tutorial **assumes you are decent at the command line and linux.** I’d
prefer not to have to also explain what shell scripts, cat, etc. are.

If you don’t have a full sync’d bitcoin node, plan to execute this tutorial in
two parts:

1.  **Before you go to bed:** Spin up your linux box and start syncing your full node.

2.  **After you wake up:** Spin up your lightning node, feed it bitcoins, and acquire
    stickers!

Got all that? Ready to be #reckless? Here we go.

## Part I

### Spin up your docker instance

First, get your linux box up and running. With Digital Ocean, it’s super-simple:
Create a droplet with > 200GB of Disk Space. Over the course of 24 hours, this
will cost you ~$2.90.

Digital Ocean has a tab at the top named ‘One-click apps’ that enables you spin
up a droplet with Docker pre-installed. After selecting all the right boxes, add
your ssh key and click the ‘Create’ button.

After SSH’ing into your new node, we can start syncing the bitcoin node. Now, I
understand there’s several implementations to choose from — `lnd` and `btcd`
seem to be getting more attention — however, I’m just going to go with the
basics here and use `bitcoind` and `lightningd`. I’m not sure it’s the best
choice, but it should be pretty easy to port this process over to different
libraries if you want.

### Start syncing the Bitcoin chain

<script src="https://gist.github.com/dougvk/db3893a87d324b47cbd0834fa323e900.js"></script>

What we’re doing here is setting up a `bitcoind (version 0.15.1)` daemon process
running on a local, private docker network. The blockchain data gets saved to
`/scratch` and we expose the ports necessary for the bitcoin and lightning nodes
to listen for peers (`8333` and `9735` respectively). We’re hiding the RPC ports
so they’re not exposed to the public.

Once you run this command, it should take about 12 hours to sync, and around
170GB of disk space. Periodically, you should tail the logs. Once the date
reaches today, you should be fully sync’d.

Next, we’re going to set up the bitcoin command line tool:

<script src="https://gist.github.com/dougvk/e00d3b976bdd474eacfb9ab7c96a8782.js"></script>

I cat’d the shell script for `bitcoin-cli` in the gist above. It contains only a
single docker command that spins up, executes the command, then cleans itself
up. Please note that since we’ve hidden the RPC ports, we must to run the CLI on
the same network as the docker processes. Once you create the script, give it a
test run. See my output above.

### Sleep!

Sleep ‘n sync.

## Part II

### Spin up your lightning node on mainnet

Once you’ve verified your bitcoin node remains sync’d with the network (just
make sure the blockheight matches the latest blockheight from e.g.
[blockchain.info](http://blockchain.info/), you should spin up your lightning
node. The process remains similar to `bitcoind`, except faster (~2 minutes).

<script src="https://gist.github.com/dougvk/9d4094c9544481495788be10e0b2d218.js"></script>

As above, so below. Notice we’re running the lightning node on the same docker
network interface as bitcoin, so the RPC clients can chat with one another. And
we’ll use the same trick with the lightning command line tool:

<script src="https://gist.github.com/dougvk/d64af97d75270af2e366e583e729c259.js"></script>

Congratulations, you’ve spun up a full lightning node! The only thing left is to
connect it to other nodes and open payment channels. This was the most difficult
part for me to figure out, but also the most rewarding.

### Feed your lightning node some Ƀcoin

I wanted $20 of stickers, so I sent `.003` bitcoin (~$30) to my lightning node.
Remember, this is all very new and buggy. Only play with what you’re willing to
lose. In order to receive delicious coin, you must generate a wallet address.

<script src="https://gist.github.com/dougvk/8bb7842de14ec839751cdf5e8921630e.js"></script>

It should take about 60 minutes for your payment to confirm. There’s my 300,000
satoshis, ready and raring to go!

### Connect to another node

{% include image.html
url="assets/images/posts/Run-your-own-mainnet-Lightning-Node/SLEEPYARK.png"
description="I clicked on SLEEPYARK to find the ID, IP, and Port. Feel free to
use any node though." %}

This is where things start to get interesting. I used [this
website](https://lnmainnet.gaben.win/) to find other mainnet lightning nodes. I
decided to connect to another highly connected node, but feel free to choose
whichever one calls to you. Remember, the higher the connectivity, the fewer
expected hops for your payment route, and the lower your fees. I chose
`SLEEPYARK` (see photo).

Next, we connect to the node then open a payment channel. N.B.: I got most of my
inspiration from the [the `c-lightning`
documentation](https://github.com/ElementsProject/lightning#opening-a-channel-on-the-bitcoin-testnet).

<script src="https://gist.github.com/dougvk/f9604ec083b0e096ff06ff22f0a9fa9e.js"></script>

Unhappily, when I followed these steps, my bitcoin node had a very low tx fee
setting, [which you can see
here](https://blockchain.info/tx/c48cbca7bc1569514e9b52dc7d4df01ae3372503cb204b82f7b2d7e3fb742b7c).
40 sat/B is no good! But somehow I got lucky.
It’s entirely possible there’s something I’m missing about opening a channel
which makes it cheaper per byte. I would err on the safe side, however, which is
why I included a step in my gist for paying 500 sat/B. [Here’s a good
site](https://p2sh.info/dashboard/db/fee-estimation) to
calculate a reasonable fee.

Ok! Once you’ve waited for 6 confirmations on the transaction for opening a
lightning channel, we can move on to the finale — paying Blockstream for
stickers. [Head over to the store](http://store.blockstream.com/), add some
stickers to your cart, and arrive at the payment page.

{% include image.html
url="assets/images/posts/Run-your-own-mainnet-Lightning-Node/pay-with-lightning-address.png"
description="This is what a lightning address looks like." %}

That long string of numbers is what’s known as a [BOLT11
address](https://github.com/lightningnetwork/lightning-rfc/blob/master/11-payment-encoding.md).
Copy and paste yours into a command to decode exactly what’s going on. Here’s
mine.

<script src="https://gist.github.com/dougvk/29723b571f2c46d045f90c35c570e7bf.js"></script>

Pretty cool, right? It contains a bunch of details about my order with
Blockstream. One of the decoded fields is `payee`, which is the node ID of the
lightning node you’re trying to route your payment. We can use this ID to
calculate how much it will cost to route a payment to that node, and then we pay
the node!

<script src="https://gist.github.com/dougvk/8d1ae818e43a105941ab32f70319995d.js"></script>

There’s an interesting thing to note here. The payment hasn’t settled to the
bitcoin blockchain. It still only exists in this payment channel and lightning
network. Yet, due to the nature of the lightning protocol, Blockstream isn’t at
risk of payment default. They own the satoshis, and there’s nothing I can do to
get them back. Pretty amazing!

Now, I await my stickers. Blockstream being the dreaded [Trusted Third
Party](http://nakamotoinstitute.org/trusted-third-parties/)…here’s hoping they
will fulfill their end of the bargain! Unhappily, meatspace ain’t as safe as
bitspace.

{% include image.html
url="assets/images/posts/Run-your-own-mainnet-Lightning-Node/payment-receipt.png"
description="A receipt for a lightning node payment." %}

### Resources

An old guide, that pointed me in the right directions:

[https://interfect.github.io/#!/posts/009-Ride-the-Lightning.md](https://interfect.github.io/#!/posts/009-Ride-the-Lightning.md).

It’s pretty dated.

In case you accidentally post something with a low fee to the network:

[https://bitcoin.stackexchange.com/questions/9046/why-is-my-transaction-not-getting-confirmed-and-what-can-i-do-about-it](https://bitcoin.stackexchange.com/questions/9046/why-is-my-transaction-not-getting-confirmed-and-what-can-i-do-about-it)
