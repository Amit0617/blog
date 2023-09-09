---
title: "Web2 to Web3 in one shot"
datePublished: Sun Aug 20 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clmbpv5qx000009l2a21vd9u0
slug: web2-to-web3-in-one-shot
tags: web-development, rest-api, ethereum, rpc, web3

---

Whenever I am introducing people to some "blockchain stuff", or come out as someone who understands it, people look at my age, and tend to ask - "How did you learn that?"

Their belief is that - resources for learning about blockchains are rare. To be honest, I am sure even at this moment there would be enough content that a human wouldn't be able to consume all of that in his whole life. However, it still looks rare mainly because you won't find many valuable things that get indexed on search engines, mostly because very few people are searching for them and this way a low market and hence lower requirements of improvements compared to other much popular things like MERN stack for web development.

> If you start from the beginning and follow their growth and evolution, you will be able to keep up with them no matter how complex they become.

It is not very difficult to learn about subjects whose applications are affecting a very small community or are in infancy and haven't received a lot of mainline media or content creators' attention. If you start from the beginning and follow their growth and evolution, you will be able to understand them no matter how complex they become.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694241011700/337687db-5dd3-4320-8867-c49f65e639eb.png align="center")

For Ethereum whenever Vitalik would have thought about creating a blockchain more advanced than Bitcoin, he probably would have written it in some form to explain what we can have in blockchains. He likely had in mind the technical requirements and potential possibilities of a decentralized ledger that could automatically execute code on a Turing-complete machine.

The next thing to consider would have been the engineering problems - how would this be achieved? Many people from various fields of knowledge, tried to develop software by breaking down those big ideas into small technical pieces and combining all those to form a final shape of software that takes given input, performs required operations and produces desired output. This software was called an **Ethereum client**.

It was available in many forms and was developed by different communities with different purposes in mind. Consequently, it was written in different languages like go, rust, C#, etc. and named geth, reth, nethermind client, etc. Geth became popular and as a result, many people used it on their computers, each computer running Geth is called a node and the network of these nodes using some form of Ethereum clients makes up the Ethereum blockchain.

Certainly, blockchain has a purpose and problems to solve but these can happen in parallel when we are introducing people to it. People would like to interact with it and are supposed to do so, for pulling blockchains into daily life purposes.

Now the question is how do you talk to this network, How will the people interact with it? Like any traditional network having servers spread across the world, you will interact with it the same way as you do already. For example, if you want to talk to a server then generally you have a URL, and that URL has different paths, parameters or request body that would allow the server to understand what you wanna communicate. You put that into the browser and the server responds as it is programmed. Similarly, you have an RPC URL (there are many companies running nodes as a service and providing access to their URL with API keys) for talking to nodes in the blockchain.

As a permissionless contributor of Ethereum Protocol Fellowship, I am working on creating a REST wrapper around those JSON-RPC calls! This would mainly provide a comfortable and easy-to-use RESTful interface to interact with Ethereum nodes.

![REST Wrapper](https://cdn.hashnode.com/res/hashnode/image/upload/v1694236361972/6943556f-1abd-45e5-ada6-712b9f1c38c9.png align="center")

Requests sent to nodes are referred to as transactions in the Ethereum space (there are a few other things too - messages or calls - which don't cause any state change) because you would be paying a certain amount of currency (ethers) to supply gas to the nodes as a computing fee, thereby causing a change in the global state of the blockchain.

This ledger (or so-called blockchain) needs to associate your transactions with you. So you are supposed to have an ID. Also, for sending currency with each transaction one needs an easy-to-access wallet which has a unique hash and hence can represent a user. These wallets were developed in the form of browser extensions to keep the overall user experience easy and smooth. These will contain a public key which will be your ID (identifier) and also a private key which will prove you as the owner of that public key.

Apart from exchanging ethers from your friends, this network can have applications running on it as it is composed of Turing-complete machines (clients or nodes). These machines give users access to programmability of money (ethers) over the network. Developers who saw some kind of opportunity/problems/needs or decentralization of entertainment like games wrote some piece of code which would execute on nodes and would update the state of the blockchain. These pieces of code are referred to as Smart Contracts in the Ethereum ecosystem (or **"chain code"** as called in the Hyperledger ecosystem, which sounds more generic for a piece of code running on blockchain). It can do computations, store values, make transactions or even create new contracts! Values like - you earned this many points in a game, or you own these game assets will be publicly provable and even transferable (if a game wants to allow that).

Smart contracts can be written in several programming languages, such as Solidity, Vyper, Yul etc. After a smart contract is deployed, it remains on the blockchain and executes as programmed whenever poked (or called). Like any piece of code, a contract can have multiple functions that perform computations. These functions can be called using `eth_call` method. Sending a request to an RPC URL with the appropriate parameters will invoke a function of a specific contract. like

```plaintext
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_call","params":[{ 
"from": "0x1923f626bb8dc025849e00f99c25fe2b2f7fb0db", 
"to": "0x07a565b7ed7d7a678680a4c162885bedbb695fe0",
"value": "0x1234",
"gas": "0x55555",
"data": "0x0"
}],"id":1}'
```

where `data` is also a hash which consists of `0x`+`first 4 bytes of Keccak hash of ASCII function signature`+`parameters in hex each padded with prefix zeros to 32 bytes`.

We wanted to develop an easy-to-use interface so that users could interact with contracts more easily. If we look at the bare bones of it, we have to make a request simply from our front-end and it will reach the nodes, will get processed by them and contracts will get executed correspondingly. Making that RPC call in complete raw form in frontend, would take certainly lots of repetition of code for each function and hence with time, libraries like `web3.js` and `ethers.js` was developed which encapsulated those RPC calls into functions and gave easy-to-program API-fied access to them with custom parameters. So that a person (who already has enough technical proficiency) can create a front-end which can talk to contracts without any need to understand how the blockchain works.

So in that order, you don't have to know everything for working on a subset of it. Just having enough of the abstraction in the mind for various concepts to build familiarity with different terms of that domain, as even black boxes is enough. The development of easy-to-use interfaces and libraries has made it easier for developers to create applications that can interact with the blockchain, without needing to understand its underlying workings.

If your curiosity arises to learn more about Web3 or Ethereum development, congratulations! People have paid me to create technical content for learning Ethereum development. I would be creating a series of articles to cover the most technical part of it for free! This was the peak of theoretical talk I could have done ever. Subscribe to get alerts.