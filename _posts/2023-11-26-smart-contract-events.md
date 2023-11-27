---
title: "Smart Contract Events"
mathjax: true
layout: post
categories: github, website
---

# Listening to smart contract events from a frontend DApp via Ethers.js

This guide will walk you through the necessary steps to listen to smart contract events via ethers.js. These steps cover the installation and configuration of essential tools and technologies.

## Prerequisites

Before proceeding, ensure you have the following prerequisites:

- Familiarity with command-line usage
- Git
- Basic understanding of smart contracts
- Knowledge of [Full Stack dApp Guide on Rootstock](https://dev.rootstock.io/guides/full-stack-dapp-on-rsk/part1-overview/)

## Step 1: Installing Node.js

Node.js is a vital component for development, especially with tools like Hardhat. Install Node.js version 12 or later:

- ### Linux

    ```shell
    sudo apt update
    sudo apt install curl git
    curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
    sudo apt install nodejs
    ```

- ### Mac OSX

    ```shell
    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.35.2/install.sh | bash
    nvm install 12
    nvm use 12
    nvm alias default 12
    npm install npm --global # Upgrade npm to the latest version
    ```

- ### Windows

    Install [Git for Windows](https://git-scm.com/download/win) and [Node.js installer for Windows](https://nodejs.org/dist/latest-v12.x/).

    > **Note:** For Windows, consider using [Windows Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/dev-environment/javascript/nodejs-on-wsl) for a better development experience.

## Step 2: Setting up RSK Local Node

To interact with RSK, you need a local node. Follow the installation instructions provided in the [RSK Devportal](https://dev.rootstock.io/quick-start/step1-install-rsk-local-node/).

> If you encounter issues, refer to [Set up RSK with Java](https://dev.rootstock.io/rsk/node/install/operating-systems/java/).

## Step 3: Setting up Hardhat

Next, set up Hardhat, a development environment for Ethereum-like networks. Navigate to the project folder and install the required packages:

```shell
cd your-project-folder
npm install
```

## Step 4: Installing ethers.js

Install ethers.js, a powerful library for Ethereum interactions:

```shell
npm install --save ethers
```

> For more details and troubleshooting, refer to the [Ethers documentation](https://docs.ethers.io/v5/getting-started/).

## Step 5: Providing a Mnemonic Phrase for RSK Testnet Deployment

To deploy on the RSK testnet, generate a secure 12-word mnemonic phrase and store it:

```shell
mnemonics > .testnet.seed-phrase
```

> **Note:** Ensure RSKj (Regtest) is running as described in the prerequisites section.

Create a `.secret` file in the project folder and store your mnemonic phrase securely.

> If you need assistance generating a mnemonic phrase, use this [resource](https://iancoleman.io/bip39/).

## Step 6: Getting The ABIs

The ABI file(s) must match your contract(s). There are a few ways to obtain ABI files:

- If you are building your own project, you will likely have access to your most current ABIs.
- If you are building for a public project, download that project to your local machine and get the ABI by using [truffle compile](https://truffleframework.com/docs/truffle/overview).
- You can also find the ABI on Etherscan, but this isn't always reliable, as the ABI that is uploaded there may be out of date.

## Step 7: Compiling and Deploying a Smart Contract

Navigate to the project folder and run the appropriate script to compile and deploy a smart contract:

```shell
npm run deploy:hardhat
npm run deploy:regtest
npm run deploy:testnet
npm run deploy:ganache
```

These scripts compile a test token (Meow Token) and deploy it to the selected network.

## Step 8: Running the DApp

To test your application, follow these steps to view it in your browser:

1. Navigate to the frontend folder:

```shell
cd ..
cd frontend
```

2. Run `index.html` in your browser using a [Live Server extension](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) in VSCode.

> **Note:** Ensure RSKj (Regtest) is running in a separate terminal.

## Final Considerations

With these steps, you've set up your development environment for RSK workshops. Utilize tools like ethers.js to listen to smart contract events from your frontend DApp and enhance your development experience.

