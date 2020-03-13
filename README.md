# Sending Money Using Python — Web3.py tutorial

_Trigger warning: The following tutorial contains elements of explicit cryptography, peer-to-peer financial services and other transgressive behaviors. The examples are meant only to illustrate the power and ease of blockchain for Python._

Hey all yall Pythoners out there!

I’m really into the Python community. Like so many, Python was my first programming language. The Python hackathon and meet-ups are awesome. And, as a bonus point, [I love Monty Python!](https://pythonspamclub.com/monty-python-and-programming-language/)

I thought I’d join all these loves together with my current job: showing developers how powerful blockchain programming can be, and how easy the skills are to pick up.

This will be a tutorial walking Python developers through the basics of [Web3.py,](https://web3py.readthedocs.io/en/stable/) a blockchain (Ethereum) library. We’ll do a lot of this from the Python interpreter. (In the next tutorial, we’ll setup a proper directory but we’re keeping things easy for now!)

_**Note: For security reasons, we’ll be sending our money over a test network. All of these same techniques can be used on the main Ethereum network.**_

Table of Contents
=================

* [Installation](#installation)
* [Setting up Connection](#setting-up-our-connection)
* [Initialization](#initialization)
* [Create an Account](#create-an-account)
* [ENS accounts](#ens-accounts)
* [Send Money](#send-money)


### Installation

We’re gonna use [pip](https://pip.pypa.io/en/stable/installing/) to install `web3.py` from our command line

```bash
$ pip install web3
```

(If you’re using virtualenv, [here’s some documentation](https://web3py.readthedocs.io/en/stable/troubleshooting.html#setup-environment) about setting up a clean environment for Web3.py)

Great! We’re ready to get started

### Setting up our Connection

Blockchain systems are decentralized networks made up of people running individual dedicated P2P software or “nodes”. It’s similar to a torrent network: To interact with the network, you either have to host a node or use a service that hosts one for you.

Since this is a basic tutorial, we’ll use a service. The most popular one is [Infura.](https://infura.io/) You can setup your own account ([instructions here](https://blog.infura.io/getting-started-with-infura-28e41844cc89/)) or you can use the Product ID below. It’s crucial you get a Project ID and API endpoint -- it will be our API endpoint to the blockchain. 

![Infura Project Dashboard](https://github.com/cooganb/web3py-tutorial/blob/master/infura-rinkeby.png)

Copy the Endpoint and **be sure to append https:// to the address.**

Once you have that, you’re ready to connect to the blockchain using Python!


### Initialization

We’ll now kick up our Python interpreter. This can vary depending on your Python installation, but is usually accomplished with running whatever keyword you typically put before a python file. For me on my Mac iTerm, it’s:

```shell
$ python
```

```
Python 3.8.2
[Clang 6.0 (clang-600.0.57)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> 

```

To check that all our setup is correct, please run your python interpreter and the following commands:

```python
>>> from web3 import Web3, HTTPProvider
>>> import json
```

The above command imports some of the main methods from `web3.py` we’re going to use to connect to the blockchain as well as the ever-faithful, native `json` library.

```python
>>> w3 = Web3(Web3.HTTPProvider("https://rinkeby.infura.io/v3/8e4cd4b220fa42d3ac2acca966fd07fa"))
```

This command creates an object called `w3`, which we initialize with our Infura API endpoint ( appended with https:// )

_Note: You need to add **HTTPS://** in front of your Infura API address, otherwise you’ll get an error!_

Now, to see if all’s gone well, we run:

```python
>>> w3.isConnected()
```

If you get `True`, congratulations! You’re connected to the blockchain!

If you get `False`, a few things you can check: 
1. If you’ve had to restart the interpreter, you have to re-import the libraries and re-initialize the variables 
2. Have you copied the Infura API key correctly? 
3. Have you installed `web3.py` and installed and imported Web3 and HTTPProvider library? 
4. Have you appended the API key with https:// ?


### Create an Account

If we’d like to send money on the blockchain, we’ll need an Ethereum account. Ethereum accounts are the main unit of identity on the Ethereum blockchain -- an account’s address is how the user is identified on the network. Underpinning the account system is a decentralized identity protocol based on [public key cryptography.](https://en.wikipedia.org/wiki/Public-key_cryptography) Essentially, identity on a blockchain network is confirmed by authentication of digital signatures from a single private key (held in secret by a single user) by its public address counterpart (held by the entire network). While it has significant user experience hurdles, it does provide a fast, peer-to-peer authentication protocol. 

Generating an account to use on the Ethereum network is super easy with `web3.py.`

_Note: In the next few steps, I’m going to break a few rules of cryptography and security. 1) I’m going to generate a private key with inadequate entropy (randomness) and 2) I’m going to post a private key online. I’m not going to use this key beyond this tutorial—it’s just for educational purposes. You should always use proper private key management, like [Geth](https://geth.ethereum.org/) or [MetaMask,](https://metamask.io/) and never share your private key publicly._

```python
>>> my_account = w3.eth.account.create(‘Nobody expects the Spanish Inquisition!’)
```

The above command uses the string input to generate the `my_account` object, which contains a private key (`my_account._private_key`) and its associated Ethereum address (`my_account._address`) . However, since this has been posted publicly, someone could generate and use the same private key. (Luckily, I’m just using it for this tutorial and only on a test blockchain network.) 

For this reason, users typically delegate private key creation and management to software called clients (like [Geth](https://geth.ethereum.org/)) or wallets (like [MetaMask](https://metamask.io/)). These projects provide an incredibly secure way to generate and handle private keys for blockchain interactions. 

### ENS Accounts

Ethereum address’ are long hexadecimal numbers. They’re nearly impossible to type or remember, so the Ethereum community created the [Ethereum Name System (ENS).](https://ens.domains/) It serves the same benefit of the Domain Name System, which replaces website server numbers ([216.58.194.46](http://216.58.194.46)) to human-readable names ([google.com](https://www.google.com)). Rather than a `.com` domain, most ENS names use a `.eth` domain.

For example, I have an Ethererum account at `0x4d3dd8471a289E820Aa9E2Dc5f437C1b2E22F598` but I’ve used ENS to map the more readable name `coogan.eth` to the address. If you type those into apps or projects that support ENS (such as `web3.py`!), it will substitute in the Ethereum hexadecimal address. I’ll show how this works in the next section. Pretty nifty!


### Send Money

For this last section, we’re going to send some money from the account we just created to my ENS-linked account, `coogan.eth`. All from the python interpreter! 

A reasonable response to sending money is, “Isn’t this just internet funny money that’s always fluctuating with a downward trajectory?” And, yes, cryptocurrency USD prices are extremely volatile. This has caused hesitation from businesses -- why would you accept a currency with an uncertain price? 

However, there’s a novel workaround built within the Ethereum ecosystem. Ethereum is called “the World Computer” because it’s a distributed system that allows developers to upload and execute their own code. Code uploaded to Ethereum in this way is called a **smart contract.** Once it's uploaded to the network, it becomes a standalone entity, with its own address, memory storage and network access. Smart contracts have given rise to a rich Ethereum developer community that is both creative and tireless in addressing challenges, like the inherent price volatility of blockchains.

In an attempt to create a stable source of value for Ethereum, a group of developers wrote and uploaded code to Ethereum called [Dai.](https://docs.ethhub.io/built-on-ethereum/open-finance/stablecoins/dai/) It’s a digital token that is always worth about $1 USD. (The technical details to achieve this stability are fascinating but beyond the scope of this tutorial -- if you’d like to read more about the mechanics [you can find more here](https://docs.ethhub.io/built-on-ethereum/open-finance/stablecoins/dai/)). Once we hold the Dai token, we can exchange it with other users for the same rate as US dollars. We’ll do that now!

_(Note: to get Dai to an account different from the one in the tutorial, you can mint your own on the testnet using [this contract address,](https://rinkeby.etherscan.io/address/0xc3dbf84abb494ce5199d5d4d815b10ec29529ff8#code) Metamask and your own account. [Follow the steps in the tutorial here](https://tokenmint.io/blog/how-to-mint-new-tokens-using-etherscan.html))_

#### Contract Instantiation

First, to interface with code that’s been uploaded by a developer to Ethereum, we need to know what methods the uploaded code exposes. `Web3.py` natively knows how to interface with the core Ethereum software, but needs guidance to interact with third-party code. We provide that guidance by providing Web3.py an [Application Binary Interface (ABI)](https://en.wikipedia.org/wiki/Application_binary_interface). Similar to an [Application Programming Interface (API),](https://en.wikipedia.org/wiki/Application_programming_interface) the ABI allows our machine to know what functions are available to us and what parameters those functions expect. ABIs are not available on the blockchain and are supplied by the developer on sites like Github or Etherscan. 

Here's the testnet Dai ABI we'll be using, please click and copy the entire code snippet:

<pre>
abi = '[{"constant":true,"inputs":[],"name":"name","outputs":[{"internalType":"string","name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"spender","type":"address"},{"internalType":"uint256","name":"value","type":"uint256"}],"name":"approve","outputs":[{"internalType":"bool","name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"totalSupply","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"from","type":"address"},{"internalType":"address","name":"to","type":"address"},{"internalType":"uint256","name":"value","type":"uint256"}],"name":"transferFrom","outputs":[{"internalType":"bool","name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"decimals","outputs":[{"internalType":"uint8","name":"","type":"uint8"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"spender","type":"address"},{"internalType":"uint256","name":"addedValue","type":"uint256"}],"name":"increaseAllowance","outputs":[{"internalType":"bool","name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"to","type":"address"},{"internalType":"uint256","name":"value","type":"uint256"}],"name":"mint","outputs":[{"internalType":"bool","name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"internalType":"address","name":"owner","type":"address"}],"name":"balanceOf","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"symbol","outputs":[{"internalType":"string","name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"spender","type":"address"},{"internalType":"uint256","name":"subtractedValue","type":"uint256"}],"name":"decreaseAllowance","outputs":[{"internalType":"bool","name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"to","type":"address"},{"internalType":"uint256","name":"value","type":"uint256"}],"name":"transfer","outputs":[{"internalType":"bool","name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"internalType":"address","name":"owner","type":"address"},{"internalType":"address","name":"spender","type":"address"}],"name":"allowance","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"inputs":[],"payable":false,"stateMutability":"nonpayable","type":"constructor"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"from","type":"address"},{"indexed":true,"internalType":"address","name":"to","type":"address"},{"indexed":false,"internalType":"uint256","name":"value","type":"uint256"}],"name":"Transfer","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"owner","type":"address"},{"indexed":true,"internalType":"address","name":"spender","type":"address"},{"indexed":false,"internalType":"uint256","name":"value","type":"uint256"}],"name":"Approval","type":"event"}]'
</pre>

We need to parse it using `json`:

```python
>>> abi = json.loads(abi)
```

We also need to tell `web3.py` where to find this code on the Ethereum network. We do so with the following code:

```python
>>> address = w3.toChecksumAddress(‘0xc3dbf84Abb494ce5199D5d4D815b10EC29529ff8’)
```

We then use the ABI and the address to instantiate a **smart contract object.** This will give us access to the functions exposed by the code:

```python
>>> dai = w3.eth.contract(address=address, abi=abi)
```
