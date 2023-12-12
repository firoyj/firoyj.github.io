---
title: "Tutorial: Listening to smart contract events with ethers.js"
mathjax: true
layout: post
categories: github, website
excerpt: "Ethers.js is a compact JavaScript library that enables developers to interact with the Ethereum blockchain. It serves as a toolkit for building decentralized applications on EVM-compatible blockchain networks.<br><br>This guide walks you through the steps to listen to smart contract events via ethers.js, covering the installation and configuration of the tools and technologies required."
---

#### Context and problem

I created this draft tutorial as an exercise to deepen my understanding of smart contracts and to test new tools as part of my documentation process. Information presented here is based on the open-source [Demo Code Snippets repository](https://github.com/rsksmart/demo-code-snippets) for [Rootstock](https://rootstock.io/), an EVM-compatible smart contracts platform. 

The document assumes the reader is a developer with some foundational knowledge of Solidity and Hardhat, and it aims to help the reader set up and use tools on Rootstock.  

> Notes:
> - As I was working on a PC, I installed [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/dev-environment/javascript/nodejs-on-wsl) to run a Linux environment and use bash commands.
> - I opted not to provide detailed instructions for `Step 2: Setting up RSK Local Node` below, as these are comprehensively explained in the page it links to. In a realistic documentation package, it would naturally link out rather than repeat the information.  

#### Contribution

In this particular instance I did not actually contribute to the project or collaborate. It was a theoretical exercise for the purposes of my own practice.

#### Impact

Theoretically, if the tutorial were applied in a real-world scenario, I believe it would help engineers understand how to use ethers.js in the context of Rootstock. Hopefully, this tutorial would serve to entice potential customers to interact with the Rootstock blockchain.

---

# Listening to smart contract events from a frontend dApp via Ethers.js

#### Introduction

[Ethers.js](https://docs.ethers.org/v5/) is a compact JavaScript library that you can use to interact with the Ethereum blockchain. It serves as a toolkit for building decentralized applications on [EVM](https://ethereum.org/en/developers/docs/evm/)-compatible blockchain networks.

In this guide, we walk you through the necessary steps to listen to smart contract events via ethers.js, covering the installation and configuration of the tools and technologies required.

### Prerequisites

Before proceeding, ensure you have the following prerequisites:

- Familiarity with command-line usage
- GitHub
- Basic understanding of smart contracts
- Knowledge of [Full Stack dApp Guide on Rootstock](https://dev.rootstock.io/guides/full-stack-dapp-on-rsk/part1-overview/)

### Step 1: Installing node.js

Node.js is a vital component for development, especially with tools such as Hardhat. Ensure you install Version 12 or later of node.js using the following:

- #### Linux

    ```shell
    sudo apt update
    sudo apt install curl git
    curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
    sudo apt install nodejs
    ```

- #### Mac OSX

    ```shell
    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.35.2/install.sh | bash
    nvm install 12
    nvm use 12
    nvm alias default 12
    npm install npm --global # Upgrade npm to the latest version
    ```

- #### Windows

    Install [Git for Windows](https://git-scm.com/download/win) and [Node.js installer for Windows](https://nodejs.org/dist/latest-v12.x/).

    > **Note:** For Windows, consider using [Windows Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/dev-environment/javascript/nodejs-on-wsl) so that your development environment is consistent with the production environment.

### Step 2: Setting up RSK local node

To interact with RSK, you need a local node. Follow the installation instructions provided in the [RSK Devportal](https://dev.rootstock.io/quick-start/step1-install-rsk-local-node/).

> If you encounter issues, refer to [Set up RSK with Java](https://dev.rootstock.io/rsk/node/install/operating-systems/java/).

### Step 3: Setting up Hardhat

Next, set up Hardhat, a development environment for Ethereum-like networks. Navigate to the project folder and install the required packages:

```shell
cd your-project-folder
npm install
```

### Step 4: Installing ethers.js

Now you're ready to install ethers.js:

```shell
npm install --save ethers
```

> For more details and troubleshooting, refer to the [Ethers documentation](https://docs.ethers.io/v5/getting-started/).

### Step 5: Providing a mnemonic phrase for RSK testnet deployment

To deploy on the RSK testnet, generate a secure 12-word mnemonic phrase and store it:

```shell
mnemonics > .testnet.seed-phrase
```

> **Note:** Ensure RSKj (Regtest) is running as described in the prerequisites section.

Create a `.secret` file in the project folder and store your mnemonic phrase securely.

> If you need assistance to generate a mnemonic phrase, use this [resource](https://iancoleman.io/bip39/).

### Step 6: Generating the ABIs

You need the Application Binary Interface (ABI) of a smart contract allows it to communicate with external applications and other smart contracts. It translates high-level information into the bytecode that the EVM understands.

Select one of the options below to obtain ABI files:

- If you are building your own project, you will likely have access to your most current ABIs.
- If you are building for a public project, download that project to your local machine and get the ABI by using [Truffle compile](https://trufflesuite.com/docs/truffle/reference/configuration/#compiler-configuration).
- You can also find the ABI on [Etherscan](https://docs.etherscan.io/api-endpoints/contracts), but this isn't always reliable, as the ABI that is uploaded there may be out of date.

### Step 7: Compiling and deploying a smart contract

Navigate to the project folder and run the appropriate script to compile and deploy a smart contract:

```shell
npm run deploy:hardhat
npm run deploy:regtest
npm run deploy:testnet
npm run deploy:ganache
```

These scripts compile a test token (Meow Token) and deploy it to the selected network.

### Step 8: Running the dApp

To test your application, follow these steps to view it in your browser:

1. Navigate to the frontend folder:

```shell
cd ..
cd frontend
```

2. Run `index.html` in your browser using a [Live server extension](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) in VSCode.

> **Note:** Ensure RSKj (Regtest) is running in a separate terminal.

### Final considerations

With these steps, you've set up your development environment for RSK workshops. Utilize ethers.js to listen to smart contract events from your frontend DApp and enhance your development experience.


