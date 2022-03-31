# private-blockchain-geth

## Interact with a command-line tool for running a private Ethereum blockchain network

> _This tutorial is meant for those with a basic knowledge of Ethereum and smart contracts, who have some knowledge of HTML and JavaScript, but who are new to dApps._
> The purpose of building this blog is to write down the detailed operation history and my memo for learning the dApps.
> If you are also interested and want to get hands dirty, just follow these steps below and have fun!~

### Prerequisites

- MacOS
- CLI with [Homebrew](https://brew.sh/) installed
- Coding IDE

### Intro & Review
In this tutorial we will be covering:

1. Install Geth
2. Run Geth
3. Create new accounts
4. Create genesis block 
5. Deploy private blockchain
6. Mining Ethereum blocks
7. Sending Tokens


#### What is Geth?
[Geth(Go Ethereum)](https://geth.ethereum.org/docs/interface/managing-your-accounts) is a command-line interface for running Ethereum nodes implemented in Go Language. Using Geth you can join the Ethereum network, transfer ether between accounts or even mine ethers.

## Getting started

### Install Geth
We should first install Geth using the following CLI command:
```linux
brew tap ethereum/ethereum
brew install ethereum
```
Then we can navigate to our favourite directory, create a folder with your favourite name (e.g. named my-private-blockchain)and navigate into the folder using the following command:
```linux
mkdir private-blockchain-geth
cd private-blockchain-geth
```
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gan4xcovcvxzn4e2g3tx.png)

### Run Geth
We can then run the following command to see if Geth has been installed successfully:
```linux
geth
```
If your CLI gets information like mine, congrats! You are on the track now. Geth is attempting to download the entire blockchain data into your PC, and the data can take up a ton of disk space. Just `Control C` to stop the command.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9odl8sa6fmbn6djxprku.png)

If you are very interested in what has happened just now, you can open a new CLI and navigate to the following directory to see the history logs:
```linux
cd Library/Ethereum/geth/
ls
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/z4gtzy4rrxed00zhs4pe.png)


### Create new accounts
Let's run the following command to create the first account on our private blockchain:
```linux
geth account new
```
This command will then prompt us to enter a password to ensure security. After entering the password two times, our **account 0** has been created.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/p3tbv8snp90c6bm75ohw.png)

Run `geth account new` again, to create our **Account 1**:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2jp0itsgggosw7y9si99.png)

### Create genesis block 

We need to create a genesis block - the first block of the blockchain - to initialize our blockchain network.

Next let's open the folder **/private-blockchain-geth** using our favourite IDE (Mine is VSCode, and currently there is no file in this directory), create a file named **genesis-block.json**, copy and paste the following code to this file: 
```json
{
    "config": {
      "chainId": 15,
      "homesteadBlock": 0,
      "eip150Block": 0,
      "eip155Block": 0,
      "eip158Block": 0,
      "byzantiumBlock": 0,
      "constantinopleBlock": 0,
      "petersburgBlock": 0,
      "ethash": {}
    },
    "difficulty": "1",
    "gasLimit": "8000000",
    "alloc": {}
  }
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jcng8gll8140b3ho3pkb.png)

The next step is to initialize the genesis block using this command:
```linux
geth --datadir . init genesis-block.json
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/c0tridtf26ap4zjbdn7s.png)

### Deploy private blockchain
The log "Successfully wrote genesis state" indicates that we have created our first block correctly. Now we can deploy our blockchain using the following command:
```linux
geth --allow-insecure-unlock --datadir . --keystore ~/Library/ethereum/keystore --networkid 4568 --http --http.addr '0.0.0.0' --http.corsdomain "*" --http.port 8502 --http.api 'personal,eth,net,web3,txpool,miner' --mine --miner.etherbase=YOUR_ACCOUNT_0_ADDRESS
```
Note: Replace `YOUR_ACCOUNT_0_ADDRESS` with your **account 0 Public address of the key**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yqdp6uccgi23fm8o6q5t.png)

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/m9ob3wzsix8zu1vf3mf8.png)

We have kicked off an interactive session that keeps printing new INFO of our blockchain. Leave it running and let's start a new CLI, navigate to the same directory **/private-blockchain-geth** and initialize Geth JavaScript console by running the following command:
```linux
geth attach geth.ipc
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kbq2gfk39pc3qkue289p.png)

### Mining Ethereum blocks
To begin mining our blockchain, we can run:
```javascript
miner.start()
```
Let's allow it to run for a while, and then stop the mining by typing:
```javascript
miner.stop()
```

Now We have rewarded some tokens for mining new blocks. We can verify this by running the following command:
```javascript
eth.getBalance(eth.accounts[0])
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2rhyc3680lxi1nxa0jqo.png)

### Sending Tokens

Run the following command to finish the authentication process of **account 0** before sending tokens:
```javascript
personal.unlockAccount(eth.accounts[0])
```
Input the password we have set to unlock **account 0**:
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/doymc04mjfh2r8mwbeym.png)

Now let's transfer tokens from **account 0** to **account 1** by running this command:
```javascript
eth.sendTransaction({from: eth.accounts[0], to: eth.accounts[1], value: 500000})
```

> **Value** here is in Wei (Where 1 ETH equals 1 x 10 ^ 18 Wei)

We should get a green transaction hash if the transaction has been done correctly.
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pi83kyutpekqyengbdr2.png)

Since we had stopped the mining process before we were creating the transaction, this transaction is added to the memory pool rather than being added to the blockchain. At this point, account 1 cannot receive the transferred token if we run:

```javascript
eth.getBalance(eth.accounts[1])
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xywjc6cntze2wta4o84l.png)

Let's start the mining process again for a while:
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hvdjno1ndn2kp7m5jphs.png)

And run the following again:
```javascript
eth.getBalance(eth.accounts[1])
```
Finally transferred tokens have been received by **account 1**:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qdsuoo985zcvl33htr99.png)

Pretty COOL!

References
https://dev.to/heydamali/a-guide-to-private-ethereum-mining-with-geth-go-ethereum-13ol

https://geth.ethereum.org/docs/interface/managing-your-accounts

https://cointelegraph.com/news/high-severity-ethereum-geth-nodes-crash-eth-hashrate-drops-etc-remains-safe

https://trufflesuite.com/tutorial/index.html
