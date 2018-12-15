---
layout: post
author: Doug von Kohorn
author_handle: dougvk
title:  "Blockchain Oracles Will Make Smart Contracts Fly"
subtitle: "Oracles will be the biggest infrastructure innovation of 2018"
date:   2018-03-10 12:00:00 -0500
categories: oracles
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


