---
description: >-
  This documentation for the Particle Network Javascript SDK will get you
  started in minutes!
---

# Web

Particle Network SDK allows you to easily integrate your app with the EVM and Solana blockchain, whether you already have a dApp integrated with web3 or are starting from scratch. Particle Network provides a smooth and delightful experience for both you and your app's users.

## Demo

Live Demo: [https://web-demo.particle.network/](https://web-demo.particle.network/)

Github: [https://github.com/Particle-Network/particle-web-demo](https://github.com/Particle-Network/particle-web-demo)

## Get Started

### Step 1: Include Particle Network SDK Script

Download Particle Network SDK to your project via Yarn

```bash
yarn add @particle-network/auth

//if you need support evm-chains
yarn add @particle-network/provider

//if you need support solana chain
yarn add @particle-network/solana-wallet
```

### Step 2: Setup Developer API Key

Before you can add Auth Service to your app, you need to create a Particle project to connect to your app. Visit [Particle Dashboard](broken-reference) to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)

```typescript
import { ParticleNetwork } from "@particle-network/auth";
import Web3 from "web3";

const pn = new ParticleNetwork({
  projectId: "xx",
  clientKey: "xx",
  appId: "xx",
  chainName: "ethereum",
  chainId: 42,
});

const evmProvider = new ParticleProvider(pn.auth);
//if you need support solana chain
const solanaWallet = new SolanaWallet(pn.auth);

window.web3 = new Web3(evmProvider);
window.web3.currentProvider.isParticleNetwork // => true
```

Your first Particle Network dApp! 🎉 You can implement web3 functionalities just like how you would normally with MetaMask.

## Web3 Integration

### Network Configuration

```typescript
import { ParticleNetwork } from "@particle-network/auth";

//ethereum Kovan test net
const pn = new ParticleNetwork({
  projectId: "xx",
  clientKey: "xx",
  appId: "xx",
  chainName: "ethereum",
  chainId: 42, //mainnet 1
});

//bsc testnet
const pn = new ParticleNetwork({
  projectId: "xx",
  clientKey: "xx",
  appId: "xx",
  chainName: "bsc",
  chainId: 97, // mainnet 56
});

//...
```

Particle support more [EVM Chains](../../node-service/evm-chains-api/#structure).

## [👉 ChainId And ChainName Configs](../../node-service/evm-chains-api/#structure)

### Get User Account

In order for web3 to work and grab the end-users' Ethereum wallet addresses, the users have to login first (similar to unlocking account in MetaMask). You can simply trigger the login for users with the web3 function call below.

```typescript
import { ParticleNetwork } from "@particle-network/auth";
import Web3 from "web3";

const pn = new ParticleNetwork({...});

// Request user login if needed, returns current user info
pn.auth.login()
```

User login modal can also be triggered through web3 accounts.

```typescript
import Web3 from "web3";

// Async functions that triggers login modal, if user not already logged in
web3.eth.getAccounts((error, accounts) => {
  if (error) throw error;
  console.log(accounts); // ['0x...']
});
```

A modal will open to ask users to sign up for an account or login with their mobile phone number or email.

### Send Transaction

If you have replaced your web3 provider with Particle Network provider, nothing needs to be changed for web3 send Ether transactions to continue working.

The Particle Network X modal will pop open and ask users to confirm their transaction once this web3 function is called on the client-side.

```typescript
const txnParams = {
    from: "0xXX",
    to: toAddress,
    value: sendValue
};
window.web3.eth.sendTransaction(txnParams, (error, txnHash) => {
    if (error) throw error;
    console.log(txnHash);
});

//for solana chain
solanWallet.signAndSendTransaction(transaction)

```

### User Signing

This is a relatively advanced use case. If you use the signed typed data JSON RPC endpoint, Particle Network will support this as well.

{% tabs %}
{% tab title="Personal Sign" %}
```typescript
import { ParticleNetwork } from "@particle-network/auth";
import Web3 from "web3";
import { Buffer } from "buffer";
import { bufferToHex, toChecksumAddress } from "ethereumjs-util";

const pn = new ParticleNetwork({...});
window.web3 = new Web3(pn.getProvider());

const text = "Hello Particle Network!";
const accounts = await window.web3.eth.getAccounts();
const from = accounts[0];
const msg = bufferToHex(Buffer.from(text, "utf8"));
const params = [msg, from];
const method = "personal_sign";

window.web3.currentProvider
  .request({
    method,
    params,
    from,
  })
  .then((result) => {
    console.log("personal_sign", result);
  })
  .catch((error) => {
    console.error("personal_sign", error);
  });
```
{% endtab %}

{% tab title="Sign Typed Data v1" %}
```typescript
import { ParticleNetwork } from "@particle-network/auth";
import Web3 from "web3";

const pn = new ParticleNetwork({...});
window.web3 = new Web3(pn.getProvider());


const accounts = await window.web3.eth.getAccounts();
const from = accounts[0];
const msg = [
    {
      type: "string",
      name: "fullName",
      value: "John Doe",
    },
    {
      type: "uint32",
      name: "userId",
      value: "1234",
    },
  ];
const params = [msg, from];
const method = "eth_signTypedData_v1";

window.web3.currentProvider
  .request({
    method,
    params,
    from,
  })
  .then((result) => {
    console.log("signTypedData result", result);
  })
  .catch((err) => {
    console.log("signTypedData error", err);
  });
```
{% endtab %}

{% tab title="Sign Typed Data v3" %}
```typescript
import { ParticleNetwork } from "@particle-network/auth";
import Web3 from "web3";

const pn = new ParticleNetwork({...});
window.web3 = new Web3(pn.getProvider());

const accounts = await window.web3.eth.getAccounts();
const from = accounts[0];
const payload = {
    types: {
      EIP712Domain: [
        { name: "name", type: "string" },
        { name: "version", type: "string" },
        { name: "verifyingContract", type: "address" },
      ],
      Person: [
        { name: "name", type: "string" },
        { name: "wallet", type: "address" },
      ],
      Mail: [
        { name: "from", type: "Person" },
        { name: "to", type: "Person" },
        { name: "contents", type: "string" },
      ],
    },
    primaryType: "Mail",
    domain: {
      name: "Ether Mail",
      version: "1",
      verifyingContract: "0xCcCCccccCCCCcCCCCCCcCcCccCcCCCcCcccccccC",
    },
    message: {
      from: {
        name: "Cow",
        wallet: "0xCD2a3d9F938E13CD947Ec05AbC7FE734Df8DD826",
      },
      to: {
        name: "Bob",
        wallet: "0xbBbBBBBbbBBBbbbBbbBbbbbBBbBbbbbBbBbbBBbB",
      },
      contents: "Hello, Bob!",
    },
  };
const params = [from, payload];
const method = "eth_signTypedData_v3";

window.web3.currentProvider
  .request({
    method,
    params,
    from,
  })
  .then((result) => {
    console.log("signTypedData_v3 result", result);
  })
  .catch((err) => {
    console.log("signTypedData_v3 error", err);
  });
```
{% endtab %}

{% tab title="Sign Typed Data v4" %}
```typescript
import { ParticleNetwork } from "@particle-network/auth";
import Web3 from "web3";

const pn = new ParticleNetwork({...});
window.web3 = new Web3(pn.getProvider());

const accounts = await window.web3.eth.getAccounts();
const from = accounts[0];
const payload = {
    domain: {
      name: "Ether Mail",
      verifyingContract: "0xCcCCccccCCCCcCCCCCCcCcCccCcCCCcCcccccccC",
      version: "1",
    },
    message: {
      contents: "Hello, Bob!",
      from: {
        name: "Cow",
        wallets: [
          "0xCD2a3d9F938E13CD947Ec05AbC7FE734Df8DD826",
          "0xDeaDbeefdEAdbeefdEadbEEFdeadbeEFdEaDbeeF",
        ],
      },
      to: [
        {
          name: "Bob",
          wallets: [
            "0xbBbBBBBbbBBBbbbBbbBbbbbBBbBbbbbBbBbbBBbB",
            "0xB0BdaBea57B0BDABeA57b0bdABEA57b0BDabEa57",
            "0xB0B0b0b0b0b0B000000000000000000000000000",
          ],
        },
      ],
    },
    primaryType: "Mail",
    types: {
      EIP712Domain: [
        { name: "name", type: "string" },
        { name: "version", type: "string" },
        { name: "verifyingContract", type: "address" },
      ],
      Group: [
        { name: "name", type: "string" },
        { name: "members", type: "Person[]" },
      ],
      Mail: [
        { name: "from", type: "Person" },
        { name: "to", type: "Person[]" },
        { name: "contents", type: "string" },
      ],
      Person: [
        { name: "name", type: "string" },
        { name: "wallets", type: "address[]" },
      ],
    },
  };
const params = [from, payload];
const method = "eth_signTypedData_v4";

window.web3.currentProvider
  .request({
    method,
    params,
    from,
  })
  .then((result) => {
    console.log("signTypedData_v4 result", result);
  })
  .catch((err) => {
    console.log("signTypedData_v4 error", err);
  });
```
{% endtab %}
{% endtabs %}

### Particle Network Native

#### Login

```typescript
import { ParticleNetwork } from "@particle-network/auth";
import Web3 from "web3";

const pn = new ParticleNetwork({...});

pn.auth.login().then(info => {
    //...
})
```

#### Logout

```typescript
import { ParticleNetwork } from "@particle-network/auth";
import Web3 from "web3";

const pn = new ParticleNetwork({...});

pn.auth.logout().then(() => {
    console.log("logout");
})
```

#### Is User Logged In

```typescript
import { ParticleNetwork } from "@particle-network/auth";

const pn = new ParticleNetwork({...});

//check user logged
pn.auth.isLogin()
```

#### Get User Info

```typescript
import { ParticleNetwork } from "@particle-network/auth";

const pn = new ParticleNetwork({...});

//get user info(token/wallet/uuid)
const info = pn.auth.userInfo();
```

#### Set Auth Theme

```typescript
import { ParticleNetwork } from "@particle-network/auth";

const pn = new ParticleNetwork({...});
pn.setAuthTheme({
  uiMode: "dark",
  displayCloseButton: true,
});

```

#### Set Chain Info

```typescript
import { ParticleNetwork } from "@particle-network/auth";

const pn = new ParticleNetwork({...});
// you can set chain info when new ParticleNetwork, or call setChainInfo
pn.setChainInfo({
    name: "polygon",
    id: 137,
})
```

## EVM Web3Modal Integration

If you use [web3modal](https://github.com/WalletConnect/web3modal) connect wallet, you can use custom provider to add particle auth.

[👉 Sample](https://web-demo.particle.network/web3Modal)

[👉 Source Code](https://github.com/Particle-Network/particle-web-demo/blob/master/src/web3Modal.ts)

## Solana Wallet-Adapter Integration

Particle Auth support Solana official component [wallet-adapter](https://github.com/solana-labs/wallet-adapter), you can quickly add Particle Auth to your DApp.

```typescript
export const Wallet: FC = () => {
    // The network can be set to 'devnet', 'testnet', or 'mainnet-beta'.
    const network = WalletAdapterNetwork.Devnet;

    // You can also provide a custom RPC endpoint.
    const endpoint = useMemo(() => clusterApiUrl(network), [network]);

    const wallets = useMemo(
        () => [
            /**
             * Select the wallets you wish to support, by instantiating wallet adapters here.
             *
             * Common adapters can be found in the npm package `@solana/wallet-adapter-wallets`.
             * That package supports tree shaking and lazy loading -- only the wallets you import
             * will be compiled into your application, and only the dependencies of wallets that
             * your users connect to will be loaded.
             */
            new ParticleAdapter(), //add particle adapter
        ],
        []
    );

    return (
        <ConnectionProvider endpoint={endpoint}>
            <WalletProvider wallets={wallets} autoConnect>
                <WalletModalProvider>
                    <WalletMultiButton />
                    <WalletDisconnectButton />
                    { /* Your app's components go here, nested within the context providers. */ }
                </WalletModalProvider>
            </WalletProvider>
        </ConnectionProvider>
    );
};
```

For detailed usage, please refer to the wallet-adapter [documentation](https://solana-labs.github.io/wallet-adapter/)





