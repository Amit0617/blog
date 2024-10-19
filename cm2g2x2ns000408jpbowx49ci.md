---
title: "Draining Null Address: An Ethereum experiment"
datePublished: Sat Oct 19 2024 11:32:15 GMT+0000 (Coordinated Universal Time)
cuid: cm2g2x2ns000408jpbowx49ci
slug: draining-null-address-an-ethereum-experiment
tags: hacking, ethereum, web3

---

### The Setup

From previous blockchain experiments, I knew that if funds were sent to a pre-calculated contract address *before* deploying the contract, those [funds could be accessed once the contract was live](https://ethereum.stackexchange.com/a/29706). So, I thought, what if I applied the same logic to the null address—perhaps the most infamous and deserted address in Ethereum’s entire ecosystem?

A contract like this was deceptively simple to write. I created two contracts, a `Withdrawer` contract (which would handle the funds) and a `Deployer` contract (which would allow me to deploy the `Withdrawer` to a specific address).

```js
//Withdrawer.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Withdrawer {
    function withdraw(uint256 _amount) public {
        payable(0xMYADDRESS).transfer(_amount);
    }
    receive() external payable {}
}
```

And the deployer:

```js
// Deployer.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
import {Withdrawer} from "./Withdrawer.sol";

contract Deployer {
    function deployWithdrawer(bytes32 salt, uint256 endowment) external returns (address payable deployed) {
        bytes memory bytecode = type(Withdrawer).creationCode;
        assembly {
            deployed := create2(endowment, add(bytecode, 32), mload(bytecode), salt)
        }
    }
    receive() external payable {}
}
```

### A Recipe for jacking Null address

I knew that for this to work, I needed the perfect combination of `salt` and `endowment` to deploy the `Withdrawer` contract to the infamous null address. So, I crafted a fuzz test in Solidity to figure out just that:

```js
// Withdrawer.t.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;

import {Test, console} from "forge-std/Test.sol";
import {Withdrawer} from "../src/Withdrawer.sol";

contract WithdrawerTest is Test {
    Withdrawer public withdrawer;

    receive() external payable {}

    function setUp() public {
        withdrawer = new Withdrawer();
        payable(withdrawer).transfer(10 ether);
    }

    function test_Withdraw() public {
        withdrawer.withdraw(1);
    }

    function testClaimNull(bytes32 salt, uint256 endowment) external {
        address payable deployed;
        bytes memory bytecode = type(Withdrawer).creationCode;
        assembly {
            deployed := create2(endowment, add(bytecode, 32), mload(bytecode), salt)
        }
        vm.assume(address(deployed) == address(0));
        require(address(deployed) == address(0), "deployed");
    }
}
```

At this point, things were starting to feel real. I had created the necessary tests to generate the right parameters for deploying to the null address. With shaking hands, I typed:

```bash
forge test --match-test testClaimNull -vvvv
```

It succeeded! My fuzz test worked flawlessly, indicating that I had the correct parameters to deploy a contract at the null address. It was at this moment that I allowed my imagination to run wild: *What would I do with all that money? Would I need to burn the funds? What if I accidentally destabilized the entire Ethereum ecosystem?*

### The First Deployment

Full of excitement, I chose Arbitrum as my test chain (I wasn’t about to throw away money on high gas fees in my quest for riches). My plan was simple:

1. Deploy the `Deployer` contract. Success!
    
2. Call `deployWithdrawer(salt, endowment)` with the magic parameters from my fuzz test. Again, success!
    
3. Check the blockchain explorer to see if I’d created the contract at the null address...
    

### A Dead End

There was... nothing.

No contract had appeared on the null address. Confused, I turned back to my test environment. I knew how my `Withdrawer` contract was supposed to behave, so I tried calling the `withdraw` function anyway, just to see what would happen.

```bash
withdraw(1 ether)
```

It [succeeded!](https://arbiscan.io/tx/0x68a001c0d4e36d47d2e1266a5a5ab7e2fcfdd4947ec0dd7c21e83706cd3ce7dc)

![](https://cdn.discordapp.com/attachments/847906692183359568/1293620509589901377/screenshot.png?ex=67148fbb&is=67133e3b&hm=4c9c9f9b24923b8cfcc92e5f95edee177d565d9dc117b2769d0feaea83ca305c& align="left")

But when I checked my balance, there was no magical influx of funds.

*Crickets.*

### What Really Happened?

At this point, I started to doubt myself. Had I misunderstood something fundamental? Was my assumption about `CREATE2` deployment wrong? After some digging and consulting with the community, the answer became clear.

I discussed it in Ethereum R&D group and got this insightful response:

> **Jochem Brouwer**: CREATE2 + CREATE returns an address (this is put on stack once you've run into CREATE\[2\] in the EVM). If the contract creation failed, then 0 is put on stack.

This explained everything. The `vm.assume(address(deployed) == address(0))` had passed because the contract creation had actually failed. When a contract creation fails, the `deployed` is assigned `0` and that becomes `address(0)` (the null address), which was why my assumption was incorrect from the beginning. I had never successfully deployed a contract there, and thus, I couldn’t withdraw any funds.

### Conclusion

So there it was—my grand plan to pull funds from the void, reduced to a small misunderstanding about EVM internals. But the experience was invaluable. Sometimes, the journey is more important than the destination (*wipes tears,* takes *copium*), and this failed experiment taught me a great deal about the workings of Ethereum’s deployment mechanism (finally! The clown puts colourful hairs wig on his head and is ready with full makeup).

In the end, the null address remains as elusive as ever, and I remain a humble developer—smarter for the experience, though perhaps not as rich as I had once dreamed.

---

Do you have any such stories? I’d love to hear your wildest one! For me, this wasn’t the first one, I have had [similar experiences](https://hashnode.com/post/clj64g3o7000909ld4p1df973).