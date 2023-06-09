---
title: "Blockchain is FAD | Reacting to technical people opinions"
datePublished: Thu Jun 08 2023 18:33:38 GMT+0000 (Coordinated Universal Time)
cuid: clinh8vw0000909mk4jg8c7ci
slug: blockchain-is-fad-reacting-to-technical-people-opinions
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1686249136540/ebd2c3a8-7ad7-4524-8a37-8150609b32e3.png
tags: software-development, technology, blockchain, ethereum, web3

---

Many technical people out there seem to completely brush aside all the web3 and blockchain stuff putting everything under same umbrella marking it as a big FAD. They often start pointing out how blockchain has never been a pioneer technology and how Distributed systems,

such as peer-to-peer file sharing networks like

* BitTorrent,
    
* content delivery networks (CDNs), and
    
* distributed databases like Apache Cassandra,
    

have been deployed and used successfully prior to the advent of blockchain.

*Bass Drop*

the idea of self-executing digital contracts with predefined conditions and actions can be traced back to the work of Nick Szabo in the 1990s.

*Bass Drop*

The cryptographic techniques and protocols such as

* hash functions,
    
* digital signatures,
    
* zero knowledge,
    

have all beeen... well-established in the field of cryptography for manyyy years.

*Bass Drop*

SILENCE

*(Hopeful music in the background)*

Acknowledging the preexistence of distributed systems helps provide context and perspective on the evolution of blockchain technology. It highlights that blockchain is an evolution and integration of existing concepts and technologies rather than a completely revolutionary or pioneering development in the domain of distributed systems.

It's only because its unique combination of existing technologies and innovative design has paved the way for novel applications and possibilities. Blockchain's impact lies in its ability to provide a transparent, immutable, and decentralized framework for various industries and use cases.

*(Music taking dramatic turn to fantasy style)*

Few things which come with decentralization and open nature(open source and public) of blockchain is modularity or compos-ability which allows to combine and interoperate different smart contracts and Dapps to create new functionalities and services.

To get the context, compare it with the current scenario of applications and services where let say if I want to build something upon already existing service then the first blocker certainly would be the centralized control over that service it completely depends upon the company or organization whether they want to allow things to be build upon them or not and accordingly they can offer APIs(Application Programming Interfaces) for developers apart from their user facing services.

The design of blockchain(ethereum, in particular) allows multiple distributed nodes consisting all the smart contracts, executing them and updating states of EVM.

Independently built contracts can easily be combined with each other to create more complex systems. Composability enables developers to leverage existing smart contracts

* as building blocks, reducing redundancy and promoting code reuse.
    
* building upon the work of others, remixing ideas, and extending functionalities,
    
* creating a vibrant ecosystem of interconnected applications, enhancing the overall value and utility,
    
* seamless integration, synergistic relationship
    
* It's like a production environment open for whole world.
    

All of this... with the decentralized i.e., permission-less environment, I don't have to ask anyone to deploy my smart contract and if I want I can do that with maintaining anonymity and have a unstoppable service which literally no organization (government, FBI, CIA) can terminate.

*Bass drop*

SILENCE

*(Change tone to serious mood)*

Okay enough of the cringe let's talk how it happens technically. With the help of a small hello world example.

In solidity, before defining smart contracts we will mention the license in machine readable format. Here I will go with an MIT License.

```solidity
// SPDX-License-Identifier:MIT
```

Then we have to mention the version to the compiler.

```solidity
pragma solidity ^0.8.0;
```

Next we will define our contract `HelloWorld` consisting of string `example_text` defined as "Hello, World!". Consider `contract` analogous to `class`.

```solidity
contract HelloWorld {
    string example_text = "Hello, World!";
}
```

In solidity, we don't have anything like `print` because there isn't any console for the real world blockchain. But we do have `event` - `emit` for creating logs in transactions. As per documentation

> Events are inheritable members of contracts. When you call them, they cause the arguments to be stored in the transaction’s log - a special data structure in the blockchain. These logs are associated with the address of the contract, are incorporated into the blockchain, and stay there as long as a block is accessible.

```solidity
    ...
    event Echo(string Text);
    ...
    emit Echo(example_text);
    ...
```

Now let's define a really simple function inside the contract and move emit inside that.

```solidity
    function say() external returns(string memory){
        emit Echo(example_text);
        return example_text;
    }
```

And finally our *The HelloWorld Contract* looks like below

```solidity
// SPDX-License-Identifier:MIT

pragma solidity ^0.8.0;

contract HelloWorld {
    string example_text = "Hello ";

    event Echo(string Text);

    function say() external returns(string memory){
        emit Echo(example_text);
        return example_text;
    }
}
```

Compiling and after deploying on RemixVM, we can call `say()` which will give following output

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686243280125/c73fbcc0-443e-4a80-a16f-496baebc0ff1.png align="center")

where decoded output consists what is returned by function and logs are set by `emit`

Now it's cool (not much) but if someone wants to make something else using it's results. Then this contract can be used as a tool by other contracts deployed by other user. Not doing same thing twice.

Just like any object oriented programming language, solidity also have `interface` which allows to call functions of other contract! They are defined with function signatures which we specifically want to call and having implementation defined in the target contract there is no need to define it again.

Look at the following example of `interface` for `HelloWorld` contract's function `say()`

```solidity
interface HWinterface {
    function say() external returns(string memory);
}
```

Now defining contract which wants to have application over another contract.

```solidity
contract Application {
}
```

We need to provide address of the contract on blockchain to tell our contract which particular implementation is of our interest.

```solidity
address HWAddress = "0xf8e81D47203A594245E36C48e151709F0C19fBe8";
// calling the function
HWInterface(HWAddress).say();
```

Now let's define a function which wants to achieve something complex working over simple blocks. So let say instead of returning same text "Hello, World!" to everyone. We want to do something custom for every person.

Instead of only "Hello, World!" we would rather say "Hello, World! I am {my address}".

*By Address I am referring to user wallet address not physical address ( ͡° ͜ʖ ͡°)*

```solidity
function complex() public returns(string memory) {
    string memory text = HWInterface(HWAddress).say();
    // store address- msg.sender gives wallet address
    // since it is address type, it needs to be converted to string type
    string memory callerAddress = toAsciiString(msg.sender);
    string memory intro = string.concat(" I am ", callerAddress);
    return string.concat(text, intro);
}
```

And finally the contract looks like below

```solidity
// SPDX-License-Identifier:MIT

pragma solidity ^0.8.0;

interface HWInterface {
    // signature of function 
    function say() external returns(string memory);
}

contract Application {
    // Address of contract we want to interact with
    address HWAddress = 0xf8e81D47203A594245E36C48e151709F0C19fBe8;

    function complex() public returns(string memory) {
        string memory text = HWInterface(HWAddress).say();
        // let's greet them now
        string memory callerAddress = toAsciiString(msg.sender);
        string memory intro = string.concat(" I am ", callerAddress);
        return string.concat(text, intro);
    }
    
    // following functions are pure and internal which means they don't
    // change the state of smart contract but come as utility or tool
    // and can be called by functions inside the contract respectively.
    function toAsciiString(address x) internal pure returns (string memory) {
        bytes memory s = new bytes(40);
        for (uint i = 0; i < 20; i++) {
            bytes1 b = bytes1(uint8(uint(uint160(x)) / (2**(8*(19 - i)))));
            bytes1 hi = bytes1(uint8(b) / 16);
            bytes1 lo = bytes1(uint8(b) - 16 * uint8(hi));
            s[2*i] = char(hi);
            s[2*i+1] = char(lo);            
        }
        return string(s);
    }

    function char(bytes1 b) internal pure returns (bytes1 c) {
        if (uint8(b) < 10) return bytes1(uint8(b) + 0x30);
        else return bytes1(uint8(b) + 0x57);
    }
}
```

I am not talking about functions like `toAsciiString()` and `char()` but still mentioning them finally if someone wants to try them, that I shamelessly stolen from [here](https://ethereum.stackexchange.com/a/8447). As the current scope of discussion has to revolve around composability. Those two can be currently digested with black-box abstraction.

Compiling and deploying with another account so that we can see if literally anyone can use functions of any contract with no relation between owners.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1686247961928/a6f8cb02-5309-4e9d-9428-3d8acdab56ac.png align="center")

We can see this time logs are empty as there wasn't any `emit` and decoded output consists of string returned by function. Address can be verified by matching it with `from` key with starting `0x` omitted which is part of all addresses.

This was composability, one of the many benefits of blockchains from [Programmer's Hiccups](https://amit0617.hashnode.dev/)