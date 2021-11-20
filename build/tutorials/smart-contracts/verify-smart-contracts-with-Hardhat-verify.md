# Verifying contracts with Hardhat verify

_This tutorial includes items from the Hardhat [getting started docs](https://hardhat.org/getting-started/)_<br>
_Inspired by [Hardhat Etherscan docs](https://github.com/nomiclabs/hardhat/tree/master/packages/hardhat-etherscan)_



### Create a project

Make sure you have Hardhat installed:
```
npm install -g hardhat
```
<br>

Create a new directory for your Hardhat project:
<br>

```zsh
mkdir Greeter
cd Greeter
```
<br>

Initialize the project:
```
hardhat init
```
<br>

Once this operation is completed, you'll now have a project structure with the following items:

* ``contracts/``: Directory for Solidity contracts<br>
* ``scripts/``: Directory for scriptable deployment files<br>
* ``test/``: Directory for test files for testing your application and contracts<br>
* ``hardhat.config.js``: Hardhat configuration file

## Compiling
Before we compile our smart contract, we must set up our environment

<br>

Run the following commands:

```zsh
npm init -y 
```
<br>

Choose yes when asked : Do you want to install this sample project's dependencies with npm? <br>
or run the following command:
<br>

```zsh 
yarn add --dev hardhat @nomiclabs/hardhat-waffle ethereum-waffle chai @nomiclabs/hardhat-ethers @nomiclabs/hardhat-etherscan ethers
```

<br>

Create a ``.env.json`` file in your project's root directory:

```json
{"mnemonic": "your-wallet-seed-phrase",
"snowtraceApiKey": "your-snowtrace-api-key"
}
```
_Get your snowtrace API key [here](https://snowtrace.io/myapikey)_
<br> <br>

Configure your ``hardhat.config.js`` file to the appropriate settings:
<br>

```js
require('@nomiclabs/hardhat-waffle');
require('@nomiclabs/hardhat-ethers');
require('@nomiclabs/hardhat-etherscan');
const { snowtraceApiKey, mnemonic } = require('./.env.json');

// This is a sample Hardhat task. To learn how to create your own go to
// https://hardhat.org/guides/create-task.html
task("accounts", "Prints the list of accounts", async () => {
  const accounts = await ethers.getSigners();

  for (const account of accounts) {
    console.log(account.address);
  }
});

// You need to export an object to set up your config
// Go to https://hardhat.org/config/ to learn more

/**
 * @type import('hardhat/config').HardhatUserConfig
 */
module.exports = {
  etherscan: {
    // Your API key for Etherscan
    // Obtain one at https://etherscan.io/
    apiKey: snowtraceApiKey,
  },
  defaultNetwork: "mainnet",
  networks: {
    localhost: {
      url: "http://127.0.0.1:8545"
    },
    hardhat: {
    },
    fuji: {
      url: "https://api.avax-test.network/ext/bc/C/rpc",
      chainId: 43113,
      gasPrice: 25000000000,
      accounts: {mnemonic: mnemonic}
    },
    mainnet: {
      url: "https://api.avax.network/ext/bc/C/rpc",
      chainId: 43114,
      gasPrice: 25000000000,
      accounts: {mnemonic: mnemonic}
    }
  },
  solidity: {
  version: "0.8.0"
  },
  paths: {
    sources: "./contracts",
    tests: "./test",
    cache: "./cache",
    artifacts: "./artifacts"
  }
};
```
_Network can be configured for mainnet deployment(see Alternatives)_ 
<br> <br>

Run the following command:

```zsh
npx hardhat compile
```
<br>

Once this operation is completed, your ``./artifacts/contracts`` folder should contain the following items:
<br>

* ``Greeter.json`` <br>
* ``console.json``<br>

### Deploy

Run the following command:
```zsh
npx hardhat run scripts/sample-script.js --network fuji
```
<br>

You should see the txn activity in your terminal<br>
![Screen Shot 2021-11-19 at 3 45 40 PM](https://user-images.githubusercontent.com/73849597/142705331-1fd17fda-0361-4ba4-a6c9-6b2c7b1fb174.png)


<br>

# Verify Smart Contracts with Hardhat verify

Hardhat verify allows users to verify contracts from the CLI

Take a look at the Fuji Testnet Explorer [here](https://testnet.snowtrace.io/) and read more about Hardhat verify [here](https://github.com/nomiclabs/hardhat/tree/master/packages/hardhat-etherscan)

If you have issues, contact us on [Discord](https://chat.avalabs.org)

## Steps
1. Run the following command:
```zsh
 npx hardhat verify <contract-address> <constructor-arguments> --network fuji 
```
<br>

2. Wait for the verification message from the CLI
![Screen Shot 2021-11-19 at 3 44 38 PM](https://user-images.githubusercontent.com/73849597/142705578-8487b5c5-0653-490d-a388-28fccefe3e78.png)



<br>

7. View the verified [contract](https://testnet.snowtrace.io/address/0x509c7810ab0EF53eF052B2a971b49A3768E1331F#code) <br>
<img width="1387" alt="Screen Shot 2021-11-19 at 3 57 35 PM" src="https://user-images.githubusercontent.com/73849597/142705413-8525dab8-393c-4a54-95b5-7141e568e86f.png">

## Mainnet deployment

Configure your ``hardhat.config.js`` file to the appropriate settings:<br>

```js
require('@nomiclabs/hardhat-waffle');
require('@nomiclabs/hardhat-ethers');
require('@nomiclabs/hardhat-etherscan');
const { snowtraceApiKey, mnemonic } = require('./.env.json');

// This is a sample Hardhat task. To learn how to create your own go to
// https://hardhat.org/guides/create-task.html
task("accounts", "Prints the list of accounts", async () => {
  const accounts = await ethers.getSigners();

  for (const account of accounts) {
    console.log(account.address);
  }
});

// You need to export an object to set up your config
// Go to https://hardhat.org/config/ to learn more

/**
 * @type import('hardhat/config').HardhatUserConfig
 */
module.exports = {
  etherscan: {
    // Your API key for Etherscan
    // Obtain one at https://etherscan.io/
    apiKey: snowtraceApiKey,
  },
  defaultNetwork: "mainnet",
  networks: {
    localhost: {
      url: "http://127.0.0.1:8545"
    },
    hardhat: {
    },
    fuji: {
      url: "https://api.avax-test.network/ext/bc/C/rpc",
      chainId: 43113,
      gasPrice: 25000000000,
      accounts: {mnemonic: mnemonic}
    },
    mainnet: {
      url: "https://api.avax.network/ext/bc/C/rpc",
      chainId: 43114,
      gasPrice: 25000000000,
      accounts: {mnemonic: mnemonic}
    }
  },
  solidity: {
  version: "0.8.0"
  },
  paths: {
    sources: "./contracts",
    tests: "./test",
    cache: "./cache",
    artifacts: "./artifacts"
  }
};
```
Run the following commands:
```zsh
npx hardhat run scripts/sample-script.js --network mainnet
```
<br>

```zsh
npx hardhat verify <contract-address> <constructor-arguments> --network mainnet
```


Thanks for reading 🔺
