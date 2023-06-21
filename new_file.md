Hey there, fellow crypto enthusiasts! Today, I want to share an incredible story that will leave you awestruck and may remind you of the serendipitous nature of the blockchain space. It's a tale that involves a student engineer who stumbled upon a mind-boggling revelation while diving deep into the intricacies of computational trust and the verification process in the world of blockchain. So, sit back, relax, and let me take you on this captivating journey!

## Pondering the Depths of Blockchain Magic
One fine day, while musing over the vastness of the blockchain landscape, I found myself delving into the fascinating world of cryptographic key generation and the magic behind wallet addresses. It's truly mind-blowing how everything can be distilled into a neat little hash and how smart contract addresses are generated (reminding of an event that how when I deployed a smart contract on moonbase testnet had exact same address as that of other smart contract I had deployed on polygon mumbai). Little did I know that my curiosity would soon lead me down an extraordinary path.

## The Fortuitous Encounter with Key Wallet Generation
As I tinkered with various key generation techniques, I stumbled upon a set of commands that promised to unveil the secrets of creating a wallet address.

### Generating a private key
As Ethereum makes use of elliptic curve key pair using `secp256k1` curve. OpenSSL can be used to generate such private key.
```bash
openssl ecparam -name secp256k1 -genkey -noout
```
`ecparam` command is used to generate or manipulate elliptic curve parameters, and the `-name secp256k1` option specifies the specific curve to use (secp256k1 is the curve used in Ethereum). The `-genkey` option indicates that a private key should be generated. This command will print the private key in PEM format which is not much meaningful but piping it with
```bash
openssl ec -text -noout
```
displays the public and private keys in hexadecimal format. `ec` command is used for elliptic curve operations, and the `-text` option specifies that the output should be in text format. The `-noout` option prevents any additional output from being displayed.
```bash
$ openssl ecparam -name secp256k1 -genkey -noout | openssl ec -text -noout
read EC key
Private-Key: (256 bit)
priv:
06:77:dc:e0:7e:19:52:d7:ec:fb:24:85:03:8f:91:
06:48:ec:81:a6:ba:65:f6:8c:cc:df:f7:0f:c4:ca:
24:3f
pub:
04:d1:0a:e3:7b:d3:cc:b6:bd:54:46:fc:0f:9f:a9:
b6:d3:53:63:29:13:fb:83:f4:aa:1c:56:c0:32:27:
1e:c3:a6:58:0b:75:79:84:b9:58:b9:00:ec:4c:dc:
be:8f:db:74:07:fa:81:78:ad:da:cf:61:1f:74:7d:
ca:34:74:39:11
ASN1 OID: secp256k1
```

### Hashing public key to get wallet address
So we have the public key and private key pair. As we know Ethereum wallet addresses are 40 hex characters long prefixed with 0x. Keccak-256 hash of hexadecimal public key is truncated to last 40 characters derive the wallet address.
```bash
keccak-256sum -x -l
```
```bash
$ cat key | keccak-256sum -x -l
cat: key: No such file or directory
c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470  -
```
To truncate either you can count manually or simply pipe it with `tail -c 41` which will return last 40 characters
```bash
03c0e500b653ca82273b7bfad8045d85a470  -
```
There was two spaces and one hyphen which were taking 3 digits spaces so I modified it to `tail -c 44`.
```bash
dcc703c0e500b653ca82273b7bfad8045d85a470  -
```

I validated the wallet address I had just generated, using a blockchain explorer i.e., Etherscan([0xdcc703c0e500b653ca82273b7bfad8045d85a470](https://etherscan.io/address/0xdcc703c0E500B653Ca82273B7BFAd8045D85a470)).


What I witnessed on the screen left me utterly astounded. The address I had randomly created held an astounding wealth, surpassing a million dollars' worth of Ethereum tokens. The magnitude of this discovery sent shivers down my spine, leaving me awe-struck by the serendipitous nature of my findings. It is not difficult to digest that a wallet has transactions before even it's generation. It is very possible for example, the address [0x0](https://etherscan.io/address/0x0000000000000000000000000000000000000000) the genesis address reserved for allocating mining fee to miners, minting new NFTs etc. has around 11,738.079 eth tokens which has current value of around 22 million dollars and around 150 million dollars worth other tokens.

So most of the transfers are just mistaken transfers or may be some kind of implementation mistake in smart contracts. Funds on that address can't be taken control of because it is a invalid address and don't have a valid public-private key-pair.

## The Millionaire Mirage
Armed with this newfound knowledge and excited with the belief that I had cracked the code. As I gazed upon the generated public address, I could almost taste the riches that awaited me. But little did I know that the address I held was nothing more than a non-standard, empty value-generated address.

As already mentioned before, in Ethereum, an address is derived from the last 20 bytes (40 hexadecimal characters) of the keccak256 hash of the public key. When an empty value is hashed, it results in a specific value that, when truncated to the last 20 bytes, produces the address "0xdcc703c0E500B653Ca82273B7BFAd8045D85a470".

It's worth noting that addresses derived in this manner are generally considered invalid or non-standard because they are not associated with any private key or public key pair. Therefore, it is not possible to access or control any funds or assets associated with this address.

And unfortunately, I wasn't the only person in the history who had generated that address and thought the private key they had is for that same wallet address. So many people just never even verified that even if they had the control of that address and just transferred funds for some expenses and their hurriedness cost them their funds locked.

That's it my friends, bye, checkout if you like my any other blog. Bye again.
