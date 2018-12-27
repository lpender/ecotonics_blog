---
layout: post
author: Doug von Kohorn
author_handle: dougvk
title:  "Blockchain Oracles Will Make Smart Contracts Fly"
subtitle: "Oracles will be the biggest infrastructure innovation of 2018"
date:   2018-03-10 12:00:00 -0500
keywords: [blockchain, truth, subjectivity, risk-taking, smart-contracts]
excerpt: "At a very high level, using an oracle means receiving data from
outside of a blockchain. Said another way, an oracle provides a connection
between real world events and a blockchain."
---

At a very high level, using an oracle means receiving data from outside of a
blockchain. Said another way, an oracle provides a connection between real world
events and a blockchain. In my opinion, all of the really interesting complex
smart contracts require outside information — financial derivatives, gambling,
stablecoins, identity…literally anything where you want to incorporate something
happening in the real world. Turns out, transparently representing real-world
events in precise digital terms is a challenge. To better conceptualize oracles,
this post will build our intuition around why it’s difficult to represent
reality, digitally.

## Smart Contracts with Street-Smarts

A smart contract is a digital agreement that enforces itself. The common example
is a vending machine. You insert money and you can either (1) get your money
back, or (2) acquire merchandise. Like a smart contract, vending machines are
programmed once and then run forever.

Smart contracts have many advantages. They’re written in unambiguous computer
code and are entirely self-contained. Unlike legal contracts, there is
absolutely no need for an external authority to make decisions — smart contracts
are run by a group of equal peers. No judges, no juries, no mediators. However,
**smart contracts are very hard to write correctly, once written are irreversible,
and their influence is limited to cyberspace**.

A smart contract is a way of making a deal by unambiguously specifying the
parties and conditions involved. **But what if some details about the past,
present, or future are unknown**? Like yesterday’s exchange rate of BTC/USD
(according to the top 5 exchanges)? Or whether your flight will land on time
tomorrow? This is where oracles enter into the equation. **Smart contracts need
oracles to resolve details that cannot be precisely known at the time the
contract is written.**

With an oracle, we can give our smart contracts street smarts. Like a vending
machine that only dispenses hot chocolate if the oracle says the temperature
dropped below freezing. Or a flight insurance agency that gives instant payouts
if the oracle says that the flight was delayed by more than 30 minutes. By
including a connection to real world events, smart contracts get much smarter.

## Truth is Subjective, Oracles Delegate Subjectivity

Outside the very narrow domain of science, people typically perceive events as
true **relative to their model of the world**:

1. What’s the temperature in Springfield, Massachusetts?
  - According to which weather stations?
  - How should the measurements be averaged and combined? Daily, hourly?
      Mean, median?
  - Should one sensor be given more weight than the others if there’s a
      disagreement?
2. Did this building burn down?
  - **Insurance agency perspective:** a building is considered incinerated only
      according to a very specific set of criteria—-they even train and deploy
      specialists to assess whether a fire passes their very high bar.
  - **Regular person perspective:** your shit’s burned down.
3. What did Google say about Trump yesterday?
  - Google shows its users different results based on their location, browsing
      history, interests, etc.
  - There’s no way to verify this claim retroactively without time-traveling to
      yesterday, so we need a way of recording search results beyond a shadow of
      a doubt.

In general, you can think of an oracle as a “human” that a smart contract can
ask for a subjective interpretation of an event. **The oracle problem resides
here: how do we resolve subjective events in a provable, consistent,
transparent, and minimally trusted way?**

## Metaphors for Oracles

## 1. A Whiteboard for Decisions

The beauty of blockchain technology is that it forces everybody to work in the
open. We take a whiteboard, we take turns writing something on the whiteboard,
and everybody can see what we’ve written. For example, anyone can download the
Bitcoin “whiteboard”, pick a wallet address, and examine every interaction
involving that wallet.

Whether taking a class, explaining an idea to coworkers, or working through a
problem — you’ve probably relied on a whiteboard to share thought processes and
decisions. A coworker will not only supply a final decision but also draw out
their decision process on a whiteboard (or something similar).

When a smart contract is placed on the Ethereum “whiteboard”, it is completely
visible to all parties of the deal. There’s no ambiguity about how the contract
will operate—**except when important events aren’t known.**

**In a smart contract, decisions about these details are specified via an
oracle.** In order to be blockchain compliant, this service must describe its
work. Where did the data come from? How was the data processed?

In the case of temperature, you might make your decision by specifying the
following details: which weather sensors, which averaging function, and what
temperature format. The point is to detail as much of the decision process as
possible. **Next, when the event occurs everything involving this decision would
be written on the whiteboard by the oracle service for all to see and
(cryptographically) verify.**

When asking an oracle a question about the world, you must be specific about
your perspective. In return, the oracle will show its steps and have
accountability.

## 2. A Judicial Court for Information

Long before computers, our society invented a way to make all contracts smart:
allow users to appeal to some Authority (court, king, mob boss, …) who would
settle the matter with finality. If the Authority were “just”, then there was
little reason to escalate a dispute to the Authority. Anyone who knew they’d
lose, wouldn’t bother trying. **For example, the merchant revolution of the middle
ages was made possible by the development of merchant courts as an Authority—
effectively, trusted oracles that allowed traders to enforce agreements
privately.**

In a smart contract, when you specify an oracle, you are choosing a ‘court’ that
will decide how your data is interpreted and what it means in the context of
your contract. If this is a smart contract for an insurance agency, then its
oracle ‘information court’ will try to resolve the question ‘did the house burn
down?’ according to the agency’s definition.

The traditional contracting system is, of course, quite expensive. A paper
contract requires courts to interpret contracts and resolve disputes. These
courts require all sorts of external enforcement — wardens, police, judges, and
attorneys. This enforcement requires taxes, which imply a whole other set of
rules, means of enforcement, and government infrastructure. And so on across all
aspects of our society. At these scales, significant corruption is inevitable.

**Just like the middle ages, these information courts, or "oracles", have the
potential to reduce much of the wasted work around enforcing traditional paper
contracts.**

## 3. A Padlock Securing the Real and Digital

To arrive at a "result," raw data often needs to be aggregated, filtered,
combined, and made consistent according to some model of the world. These are
processes too expensive to occur on the blockchain, **so they should be performed
off-chain and secured with a cryptographic padlock.**

For example, let’s say we want to know yesterday’s temperature. How can an
oracle prove "what was yesterday’s temperature?" to others without
time-traveling? Somehow, oracles must return an acceptable padlock that ties
these details down in a way that can’t be forged or fabricated. There’s no
single way to do this, and it’s an area of ongoing research.

We do know that the padlock must be cryptographic in its nature. The lock should
specify everything that goes into the final result as precisely as possible. If
a random person wants to know what happened and why, they should be able to run
and explore every detail of of the oracle’s decision on their computer — without
recourse to anything like a discussion with the author.

## 4. Institutions with Skin in the Game

Most infrastructure we use today implicitly trusts a sprawling stack of
institutions that don’t suffer risk from their own failure. These institutions
hide risks in ways that are impossible to discover until it is too late. The
2008 bailouts of the ratings agencies and the banks, Equifax leaking 150 million
social security numbers, and thousands more. Users have no recourse, and the
institutions remain unharmed.

> This attack could have been prevent post-2013, when the
[@IETF](https://twitter.com/IETF) considered including mandatory encryption as
part of the new HTTP/2.0 standard. But they blocked it despite explicit warnings
that without that protection, users would soon face exactly the attacks we see
today. [https://t.co/n6Ezz5zil4](https://t.co/n6Ezz5zil4)

-[@Snowden](https://twitter.com/Snowden/status/972115183572267009)

<figcaption>One of the many asymmetries between users and institutions today.
</figcaption>

Unhappily for us peasants, bureaucracy is a construction by which an institution
is conveniently separated from the consequences of its actions. But there’s
potential for blockchain-based institutions to reduce bureaucratic bloat by
forcing everything to operate in a completely transparent, untrustworthy, and
anonymous environment. As digital technologies have reduced the barrier to entry
for information creation and distribution, it has become extremely important to
be able to authenticate information as originating from a known, trusted source.

For oracles, this means we must make cheap talk expensive and force institutions
to share in the risk of publishing bad information. Skin in the game means
consequences when you are wrong as much as when you are right. Oracles try to do
this today with some combination of staking money on their claims, building
reputation over time, signing data, and, in worst-case scenarios, third-party
arbitration. There’s no single correct way to structure these interactions. Just
like relationships in the real world, oracles will have differing codes of
ethics that depend on commercial context.

The idea of skin in the game comes down to this: if you give an opinion, and
someone follows it, you are morally obligated to be exposed to its consequences.
This localizes risk and makes the relationship more robust to failure. Exposing
both parties to similar risks keeps the system from rotting. **An oracle should
be expected to share in the consequences of its determinations — skin in the
game is necessary for fairness, commercial efficiency, and risk management.**

## Localizing our Future: Symmetric Relationships

Metaphors aren’t just the basis of our language — they’re also how we
conceptualize the world. They leave out the hairy details and give structure to
our thoughts. I hope these metaphors — a whiteboard, a court system, a padlock,
and institutions with skin in the game — will ground your understanding as you
dive deeper into oracles.

We all have a stake in the truth. Society functions on an assumption that people
stake their reputation on their word. In this way, truth prevails over
lying — and, for the most part, it does. If it didn’t, relationships would have
a short shelf life and commerce would cease. All of us depend on truth because
when honesty is lacking, we suffer, and society suffers. With improved tools for
scaling trust and truth, we will do better.

> (Thanks to Nassim Nicholas Taleb for the Skin in the Game metaphor. A good
> deal
of this post is inspired by his latest work. All errors of interpretation are my
own.


