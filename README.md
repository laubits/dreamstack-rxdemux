<p align="center">
	</a>
	<a href="https://git.io/col">
		<img src="https://img.shields.io/badge/%E2%9C%93-collaborative_etiquette-brightgreen.svg" alt="Collaborative Etiquette">
	</a>
  <a href="https://developers.blockmatic.io">
		<img src="https://img.shields.io/badge/code%20style-blockmatic-brightgreen.svg" alt="Blockmatic Standard">
	</a>
	<a href="https://twitter.com/intent/follow?screen_name=telosdreamstack">
		<img src="https://img.shields.io/twitter/follow/telosdreamstack.svg?style=social&logo=twitter" alt="Follow on Twitter" />
	</a>
	<a href="https://t.me/telosdreamstack">
		<img src="https://img.shields.io/badge/-Chat%20on%20Telegram-blue?style=social&logo=telegram" alt="Chat on Telegram">
	</a>
</p>

# Reactive EOSIO Demux (RxDemux)

Reactive deterministic event-sourced state and side effect handling for blockchain applications.

## Disclaimer

This is a work in progress. Feel free to reach us out on telegram for clarification.
https://t.me/telosdreamstack

## Features

- subscribe to real-time action events and state changes.
- automated eosio microfork handling.
- postgres hasura graphql updater package.
- functional reactive programming code style with TypeScript type safety.

## Demux Pattern

Demux is a backend infrastructure pattern for sourcing blockchain events to deterministically update queryable datastores and trigger side effects. In computer science, a deterministic algorithm is an algorithm which, given a particular input, will always produce the same output, with the underlying machine always passing through the same sequence of states.

Storing and retrieving indexed data is something that developers have commonly utilized for decades. The ability to search, sort, filter, etc. are all easily done in traditional database environments, but is something that is missed when working directly with the inherently limited query interface of blockchain nodes.

RxDemux allows deterministic updates of queryable data stores and event triggers through real-time subscriptions to EOSIO blockchain actions and state deltas using the state_history_plugin web socket and RxJS observable streams.

With the Demux pattern, we can separate concerns and utilize the technology more appropriate to solve them. The blockchain provides decentralized consensus and high security and it's treated at the single source of truth.

Storing blockchain historical data in queryable datastores is useful because:

* The query interface used to retrieve the indexed data is directly from the blockchain is limited. Complex data requirements can mean you either have to make an excessive number of queries and process the data on the client, or you must store additional derivative data on the blockchain itself.
* Scaling your query load means creating more blockchain endpoint nodes, which can be very expensive.

Demux solves these problems by off-loading queries to any persistence layer that you want. By deriving all state from the blockchain we gain the following benefits:

* If the accumulated datastore is lost or deleted, it may be regenerated by replaying blockchain actions
* As long as application code is open source, and the blockchain is public, all application state can be audited
* No need to maintain multiple ways of updating state (submitting transactions is the sole way)

RxDemux is a reactive alternative to the [DemuxJS](https://github.com/EOSIO/demux-js) library developed by Block.one

## EOSIO State History Plugin

State history plugin for nodeos is part of EOSIO distribution. It stores all transaction traces and table deltas in compressed archive files and provides a websocket interface for querying the traces and deltas for a specific block interval. The plugin listens on a socket for applications to connect and sends blockchain data back based on the plugin options specified when starting nodeos. If the end block number is in the future, the plugin delivers new traces as they become available in real-time. It has also an option to deliver only traces from irreversible blocks. 

The data that is exported via this websocket interface is in binary form. The plugin is designed to spend as little as possible CPU time on interpreting the data, so it’s the job of the client to decode the output and process into a usable form. Action arguments are delivered in serialized form, in the same binary form as they are submitted to the smart contract. The same applies to table deltas: the plugin does not try to interpret the contract table contents, it just sends you modified rows as they are stored in server memory.

So, to be able to interpret its output, the client software has to have copies of ABI that the smart contracts are publishing. The ABI is delivered in the same binary form as part of contract data changes when setabi action is executed. As smart contracts and ABI get updated, the client software needs to maintain relevant revisions of ABI to be able to interpret the history data.

[EOSIO Developers Documentation for state_history_plugin](https://developers.eos.io/manuals/eos/latest/nodeos/plugins/state_history_plugin/index)

[Source code of state_history_plugin](https://github.com/EOSIO/eos/blob/master/plugins/state_history_plugin/state_history_plugin.cpp)

## Reactive Programming & RXJS

RxJS is a library for functional reactive programming in JavaScript using Observables to make easier to compose asynchronous or callback-based code. Observables are a representation of any set of values over any amount of time. This is the most basic building block of RxJS. 

<p align="center">
	<img src="./assets/observer-desing-pattern.png" width="600">
</p>

RxJS (and reactive programming in general) can be thought of as writing assembly lines in your software applications. It allows you to write software that is reusable, configurable, and asynchronous. These assembly lines can be chained together, split apart, configured slightly differently, or just used without any modification at all. 

[Intro to RxJS](https://www.youtube.com/watch?v=flj-OprlogY)   
[RxJS: Mastering the Operators](https://www.youtube.com/watch?v=ou3oRHaUpQA)   
[learnrxjs.io](https://www.learnrxjs.io/)   

## EOSIO Microforks

Due to the distributed nature of the blockchain and its high speed, there are times in which block producers can get slightly out of sync because of subsecond network latency, creating two or more competing versions of the original chain that share the same history up to a certain point. The blockchain code is programmed to quickly solve this problem taking the largest version as the ‘winning’ version of the blockchain, discarding all other versions. ( Chainbase, chain_kv, chainlib, chain plugin, fork db, producer plugin and net plugin are involved with switching forks internally. )

When this happens, and happens often, your queryable read databases have to rollback discarded blocks too. rxDemux will notify when these microforks occur, the hasura updater will rollback the changes on your read postgres database and all your application clients listening to postgres state through graphql subscription will get new state in real-time as well.

[Everything You Need to Know About Microforks by EOS Canada](https://www.youtube.com/watch?v=n-LRxhFEQg4)

## Getting Started

...

## TELOS

Best in class C++ EOSIO protocol technology providing fast, scalable and eco-friendly blockchain augmented by a growing library of Telos services and innovations. Free accounts, work proposals, dapp grants, decentralized exchange, dstor, bancor protocol, the telos community and foundation.

[Learn more](https://docs.telosdreamstack.io/smart-contracts/telos-blockchain)

## TELOS DreamStack

This repository is part of the TELOS DreamStack Project which provides a set of guidelines, tools, and starters to speed up dApp development following best practices for security, performance, and maintainability of your application.

The code style convention aims to maximize reusability and facilitate collaboration. We have chosen the stack carefully so that it allows us to write robust and performant applications with a more concise and readable code.

## Contributing

Read the [contributing guidelines](https://developers.blockmatic.io) for details.

## Blockmatic

Blockmatic is building a robust ecosystem of people and tools for the development of blockchain applications.

[blockmatic.io](https://blockmatic.io)

<!-- Please don't remove this: Grab your social icons from https://github.com/carlsednaoui/gitsocial -->

<!-- display the social media buttons in your README -->

[![Blockmatic Twitter][1.1]][1]
[![Blockmatic Facebook][2.1]][2]
[![Blockmatic Github][3.1]][3]

<!-- links to social media icons -->
<!-- no need to change these -->

<!-- icons with padding -->

[1.1]: http://i.imgur.com/tXSoThF.png (twitter icon with padding)
[2.1]: http://i.imgur.com/P3YfQoD.png (facebook icon with padding)
[3.1]: http://i.imgur.com/0o48UoR.png (github icon with padding)

<!-- icons without padding -->

[1.2]: http://i.imgur.com/wWzX9uB.png (twitter icon without padding)
[2.2]: http://i.imgur.com/fep1WsG.png (facebook icon without padding)
[3.2]: http://i.imgur.com/9I6NRUm.png (github icon without padding)


<!-- links to your social media accounts -->
<!-- update these accordingly -->

[1]: http://www.twitter.com/blockmatic_io
[2]: http://fb.me/blockmatic.io
[3]: http://www.github.com/blockmatic

<!-- Please don't remove this: Grab your social icons from https://github.com/carlsednaoui/gitsocial -->

