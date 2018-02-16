# CryptoTickets ICO Contract

Please see below description of [CryptoTickets ICO][cryptoTickets] smart contract developed by [Phenom.Team][phenom].

## Overview
CryptoTickets Token smart-contract is structured upon [ERC20 standard](erc20).
One of distinctive features of the smart-contract is the fact that token price is fixed and pegged to USD instead of ETH which protects investors from volatility risks of ETH currency. This technical feature is made possible by usage of Manager that updates ETH/USD actual exchange rate in the smart contract every 30 minutes. The token price is set to $0.4 apiece.

Ethereum is not the only currency investors can use when investing, they can also opt to purchase CryptoTickets tokens with BTC, LTC, BCC and even USD (in debit cards). Processing of transactions in these currencies is enabled by automated platform, which processes every single incoming transaction and calculates it's USD equivalent. It also tracks number of confirmations of every single transaction and emits tokens to Ethereum address of an investor, he specified in his personal profile web page. Emission itself is processed by bringing up of buyForInvestor method directly from cotroller-addresses (controllersOnly). In order to ensure smooth real-time emission of CryptoTickets tokens, a unique sharding technology developed by  [Phenom.Team][phenom] is used. The basics of the technology boils down to the procedure wherein the whole emission process is distributed among three controller-addresses, which allows to perform fast real-time token emission  even when the whole Ethereum network is overloaded.


## The Crowdsale Specification
*	CryptoTickets token is ERC-20 compliant.
*   Allocation of CryptoTickets tokens goes in the following way:
    * Bounty: 2%
    * Advisors: 3,5%
    * Team: 15%
    * Storage: 3%
    * Ico and Private offer: 76,5%


## Code

#### CryptoTicketsICO Functions

**Fallback function**
```cs
function() external payable
```
Fallback function calls function buy(address _investor, uint _tktValue) to create tokens when investor sends ETH directly to ICO smart contract address.

**setRate**
```cs
function setRate(uint _RateEth) external managerOnly
```
Set ETH/USD exchange rate and update token price.

**startIco**
```cs
function startIco() external managerOnly
```
Set ICO status to IcoStarted.

**pauseIco**
```cs
function pauseIco() external managerOnly
```
Set Ico status to IcoPaused.

**finishIco**
```cs
function finishIco() external managerOnly
```
Set ICO status to IcoFinished. Mint tokens for BountyFund, AdvisorsFund, ItdFund, StorageFund. Defrost tokens.

**buyForInvestor**
```cs
function buyForInvestor(address _investor,uint _tktValue,string _txHash) external controllersOnly
```
buyForInvestor function is called by one of controllers. It uses buy(address _investor, uint _tktValue) function to allocate tokens to investors who make a deposit in non-ETH currencies.

**buyTokens**
```cs
function buy(address _investor, uint _tktValue) internal startedOnly
```
internal function that called by buyForInvestor and fallback functions

**getBonus**
```cs
function getBonus(uint _value) public constant returns (uint)
```
function that calculates bonus

**replaceToken**
```cs
function replaceToken(address _investor) managerOnly
```
replace tokens from presale


**withdrawEther**
```cs
function withdrawEther(uint256 _value) external managerOnly
```
allows to withdraw ether from smart contract, when ICO is finished

#### CryptoTicketsIco Events

**LogStartICO**
```cs
event LogStartICO()
```
**LogPauseICO**
```cs
event LogPauseICO()
```
**LogFinishICO**
```cs
event LogFinishICO(address bountyFund, address advisorsFund, address ItdFund, address storageFund)
```
**LogBuyForInvestor**
```cs
event LogBuyForInvestor(address investor, uint tktValue, string txHash)
```
**LogReplaceToken**
```cs
event LogReplaceToken(address investor, uint tktValue)
```

## Prerequisites
1. nodejs, and make sure it's version above 8.0.0
2. npm
3. truffle
4. testrpc

## Run tests
1. run `testrpc -a 20 -l 500000000000000` in terminal
2. run `truffle test` in another terminal to execute tests.

## Deploy contract with truffle to network

1. Change parameters of constructor in migrations/2_deploy_contracts.js
2. Add network to truffle.js:
    live: {
      host: "localhost",
      port: 8545, //port where node is running
      network_id: "1" ,// Match any network id. 1- live network, 2 - ropsten, 3 - rinkeby
      gas: 7721975 //optional
    }
3. In console in ico-smartcontract-cryptotickets then run: truffle compile
4. In geth console unlock wallet which will be used for deploy
5. Then use command: truffle migrate --network live

   More information about deploy with truffle [here][truffle]
## Collaborators

* **[Alex Smirnov](https://github.com/AlekseiSmirnov)**
* **[Dmitriy Pukhov](https://github.com/puhoshville)**
* **[Kate Krishtopa](https://github.com/Krishtopa)**


[cryptoTickets]: https://crypto.tickets/
[phenom]: https://phenom.team/
[erc20]: https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20-token-standard.md
[truffle]: http://truffleframework.com/tutorials/deploying-to-the-live-network
