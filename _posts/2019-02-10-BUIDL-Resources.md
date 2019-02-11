---
  layout: post
  author: dougvk
  date:   2019-02-09 12:00:00 -0500
  title: "Rhombus Resources for Denver BUIDLWeek"
  tags: [Blockchain, ETHDenver]
  excerpt: "Here's what Rhombus' got cooked up for y'all for #BUIDLweek"
---

Rhombus Resources for Denver BUIDLWeek
--------------------------------------

Fellow BUIDLers,

Let's cut to the chase - here's what Rhombus' got cooked up for y'all for #BUIDLweek:

### Local development and testing:

[We've released a truffle box](https://github.com/RhombusNetwork/lighthouse-local) that includes our Lighthouse contracts as well as an example dApp (including a full suite of tests -- solidity and javascript). We hope this makes your trip from idea to Rinkeby (or goerli) a smooth one!

### Available Oracle feeds:

You can read more about the return value data types [here](https://github.com/RhombusNetwork/rhombus-public/blob/master/readme.md#possible-data-types).

### Hourly updates provided by Dow Jones

-   \<DECIMAL> [AMZN](https://rinkeby.etherscan.io/address/0x0dc46Ead780eE9D432477255D695C249fDa7A566), [NVDA](https://rinkeby.etherscan.io/address/0x114C97deD229F8390fB0900e3D98dDe57aBa01d2), [TSLA](https://rinkeby.etherscan.io/address/0x6C7a951d27B0fCdcE217d621c5CeE74DAF5d6b28) stock price

-   \<DECIMAL> USD exchange rate to [GBP](https://rinkeby.etherscan.io/address/0x15d1857D57735aBD48C50E5c7a54d0aCBfB88B13), [EUR](https://rinkeby.etherscan.io/address/0x383B502E0DC5f608BC4573fB6fF40bf4aed4fD3f), [JPY](https://rinkeby.etherscan.io/address/0x47868830Fe135AE4795173fd9255EEd0AED1b7Cd)

### Hourly updates provided by Alethio

-   \<DECIMAL> [Last hour ETH transaction volume](https://rinkeby.etherscan.io/address/0x5d8dca07D30dC77ac1B3BBF6bC639A0335658166)

-   \<INT> [Last hour number of ETH transactions on-chain](https://rinkeby.etherscan.io/address/0xED11119559EBe416d2637f1656ba77d9cE885C06)

### Hourly updates provided by Rhombus

-   \<DECIMAL> [USD/ETH exchange rate](https://rinkeby.etherscan.io/address/0x9aED5cA00A44682cDC488b7778728a728717f7e3)

-   \<INT> [USD/ETH exchange rate](https://rinkeby.etherscan.io/address/0x6C5F72e1A3C5E5e384AB66CFb7A4D628895c2eBA)

-   \<DECIMAL> [$0.10USD -> ETH exchange rate](https://rinkeby.etherscan.io/address/0x566c3d00f4de34cce6f1452f4bb6c189801ba309)

-   \<DECIMAL> [USD/GOLD exchange rate (per troy ounce)](https://rinkeby.etherscan.io/address/0xe93ab27a89a415124c680906f00bca3c228891ab)

-   \<DECIMAL> [USD/GOLD exchange rate (per gram)](https://rinkeby.etherscan.io/address/0xEa648fd60D6499C25AA7c7E4657EC318E9C79298)

-   \<INT> Temperature (degrees Fahrenheit) of [SF](https://rinkeby.etherscan.io/address/0x1aB68d567813929C5dC220Ceee4DFe0C9D11BC8f), [NYC](https://rinkeby.etherscan.io/address/0x8033325A4f86AE2bA59F24ebc594129FA514E4A5), and [Boston](https://rinkeby.etherscan.io/address/0x70Df6230cEB03Ee597Df78e9896988122f25f0dE)

-   \<INT> [Random number generator (1 through 6)](https://rinkeby.etherscan.io/address/0x613d2159db9ca2fbb15670286900ad6c1c79cc9a)

### 10 minute updates provided by Rhombus

-   \<STRING> [Random color generator (ROYGBIV)](https://rinkeby.etherscan.io/address/0x46b0EA3449C3D583c82A19028C35B36357dD2E2B)

### Demos (more details at our [public repo](https://github.com/RhombusNetwork/rhombus-public)):

-   [Daily Allowance](https://github.com/RhombusNetwork/rhombus-public/tree/master/DailyAllowance) [subscription dApp]

Use the 'poke' feature of our Lighthouse contract to notify a dApp when a new value has been sent to the Lighthouse. Here, the new value is the exchange rate for ten cents of USD denominated in ETH. The dApp uses this exchange rate to pay out one dollar to a given contract every day. [See the live dApp here](https://rinkeby.etherscan.io/address/0x569702e8f49d802a8816726491a3a9070a001d03#code).

-   [Petting Zoo video feed](https://github.com/RhombusNetwork/rhombus-public/tree/master/TenCentsAMinute) [subscription dApp]

An imaginary petting zoo uses the Ethereum network to manage its subscriptions. Imagine your users want to purchase per-minute subscriptions to a live feed. Each minute costs ten cents. The dApp uses a Rhombus oracle to manage who's paid and for how long. [See the live dApp here](https://rinkeby.etherscan.io/address/0x16cba6226f0a888d4d9478977df98a3fd0041f58#code).

-   [Minting unique Gold Bars](https://github.com/RhombusNetwork/rhombus-public/tree/master/barShop) [ERC721 dApp]

A company is in the business of offering gold for investment purposes both in the form of fungible tokens backed by 1kg gold bars (no holding is tied to any specific bars) and non-fungible tokens where each token represents a unique gold bar. Using a Rhombus oracle, the dApp can negotiate with the client using up-to-the minute Gold price quotes. [See the live dApp here.](https://rinkeby.etherscan.io/address/0x16d5d4e29be197ef1f7ac414e59833edc240bdc8#code)

We absolutely cannot wait to see what you build!

-The Rhombus Team
