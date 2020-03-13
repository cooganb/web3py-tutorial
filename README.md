# Sending Money Using Python — Web3.py tutorial

_Trigger warning: The following tutorial contains elements of explicit cryptography, peer-to-peer financial services and other transgressive behaviors. The examples are meant only to illustrate the power and ease of blockchain for Python._

Hey all yall Pythoners out there!

I’m really into the Python community. Like so many, Python was my first programming language. The Python hackathon and meet-ups are awesome. And, as a bonus point, [I love Monty Python!](https://pythonspamclub.com/monty-python-and-programming-language/)

I thought I’d join all these loves together with my current job: showing developers how powerful blockchain programming can be, and how easy the skills are to pick up.

This will be a tutorial walking Python developers through the basics of [Web3.py,](https://web3py.readthedocs.io/en/stable/) a blockchain (Ethereum) library. We’ll do a lot of this from the Python interpreter. (In the next tutorial, we’ll setup a proper directory but we’re keeping things easy for now!)

_*Note: For security reasons, we’ll be sending our money over a test network. All of these same techniques can be used on the main Ethereum network.*_

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



Copy the Endpoint and *be sure to append https:// to the address.*

Once you have that, you’re ready to connect to the blockchain using Python!


### Initialization

### Create an Account

### ENS Accounts

### Send Money
