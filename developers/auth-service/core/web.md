---
description: >-
  Auth Core is a library that makes it easy to add wallet connection to your
  dapp. It's intuitive, responsive and customizable.
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Web

## Introduction

Auth Core can be integrated with any wallet, platform, chain and social login. You can even plug in your existing auth setup.

### **Wallet management**

Auth Core will automatically create a crypto wallet without a private key. The security is guaranteed by **MPC (Multi-Party Computing) - based threshold signatures**, which is ahead of other technologies.

### **Customizable**

You can tweak the Auth Core to match your branding. You can pick from a few pre-defined accent colors and border-radius configurations. For more advanced use cases, you can provide in a fully custom theme, render your own button, and omit certain features. Dark mode included.

### **Industry standards**

For better interoperability with most products, we support use with [web3.js](https://github.com/web3/web3.js), [ethers.js](https://github.com/ethers-io/ethers.js), and [viem](https://github.com/wagmi-dev/viem) — the most commonly used libraries in the space.

## Installation

Get up and running with Auth Core.

### Quick start

You can scaffold a new AuthCore + React app with one of the following commands, using your package manager of choice:

```shell

npm init @particle-network/auth-core-modal@latest
# or
pnpm create @particle-network/auth-core-modal@latest
# or
yarn create @particle-network/auth-core-modal

```

This will prompt you for a project name, generate a new directory containing a boilerplate project, and install all required dependencies.

Alternatively, you can manually integrate Auth Core into your existing project.

#### Manual setup

```shell

yarn add @particle-network/auth-core-modal

```

### **Additional build tooling setup**

The [Webpack v5](https://webpack.js.org/blog/2020-10-10-webpack-5-release/) bundler used by Create React App [no longer provides Node polyfills](https://webpack.js.org/blog/2020-10-10-webpack-5-release/#automatic-nodejs-polyfills-removed), so you'll need to include these modules yourself to satisfy AuthCore's peer dependencies. add the below code to webpack config.

```javascript

{
    resolve: {
        fallback: {
            http: require.resolve('stream-http'),
            https: require.resolve('https-browserify'),
            stream: require.resolve('stream-browserify'),
            assert: require.resolve('assert'),
            zlib: require.resolve('browserify-zlib'),
            url: require.resolve('url'),
        },
        plugins:[
            new webpack.ProvidePlugin({
                Buffer: ["buffer", "Buffer"],
            })
        ]
    }
}

```

And install dependence packages.

```sh

yarn add stream-http https-browserify stream-browserify assert browserify-zlib url

```

### **Configure**

Init Auth Core with Particle project config, you can get the config from [Particle Dashboard](https://dashboard.particle.network/).

Wrap your application with `AuthCoreContextProvider`.

<pre class="language-typescript"><code class="lang-typescript">
<strong>import { AuthCoreContextProvider } from '@particle-network/auth-core-modal';
</strong>
const App = () => {
    // replace 'xxxx' with particle project config.
    return (
        &#x3C;AuthCoreContextProvider
            options={{
                projectId: 'xxxx',
                clientKey: 'xxxx',
                appId: 'xxxx',
                promptSettingConfig: { //optional
                    //set payment password prompt: none, first, every, everyAndNotSkip.
                    promptPaymentPasswordSettingWhenSign: PromptSettingType.first,
                    //set master password prompt: none, first, every, everyAndNotSkip.
                    promptMasterPasswordSettingWhenLogin: PromptSettingType.first,
                },
                erc4337: { // optional
                    name: 'SIMPLE',  // support 'SIMPLE', 'CYBERCONNECT', 'BICONOMY'
                    version: '1.0.0',
                },
                wallet: {
                    visible: true,  //show wallet entrance when connect success.
                }
            }}
        >
            &#x3C;YourApp />
        &#x3C;/AuthCoreContextProvider>
    );
};

</code></pre>

#### ERC-4337

Auth Core Modal will display smart account address and badge when set `erc4337` param, you can use smart account send gasless transaction, refer to [Account Abstraction](../../account-abstraction/web.md).

#### Embedded Wallet

A fully functional wallet will be embedded in your web page when you set param `wallet`, and you can custom Wallet UI style to match your branding.&#x20;



Now, you can use Auth Core in your app.

## Usage

Auth Core is implemented based on React, provides a rich set of hook functions and custom components.

### Connect Wallet

Custom you own connect button and call `connect` function to connect Auth Core.

<pre class="language-typescript"><code class="lang-typescript"><strong>
</strong><strong>import { useConnect } from '@particle-network/auth-core-modal';
</strong>
const { connect, disconnect, connectionStatus, requestConnectCaptcha } = useConnect();

// open modal and connect 
const userInfo = await connect();

// connect with email.
const userInfo = await connect({
    email: 'xxx@xx.com',
    code: 'xxxxxx', // optional
});

// connect with phone.
const userInfo = await connect({
    phone: '+1xxxxxxxx', //E.164 format
    code: 'xxxxxx', // optional
});

// connect with social type.
const userInfo = await connect({
    socialType: 'google',
<strong>    prompt: 'select_account', //optional, only google, discord and microsoft support it.
</strong>});

// connect with JWT.
const userInfo = await connect({
    jwt: 'xxxxxxxxxx'
});

</code></pre>

For using a email/phone connect wallet, you can custom UI and get `code` by `requestConnectCaptcha`.

For using social connect wallet, you can specify the `prompt` param.  `prompt`  is A space-delimited list of string values that specifies whether the authorization server prompts the user for reauthentication and consent. The possible values are:

*   `none`

    The authorization server does not display any authentication or user consent screens; it will return an error if the user is not already authenticated and has not pre-configured consent for the requested scopes. You can use `none` to check for existing authentication and/or consent.
*   `consent`

    The authorization server prompts the user for consent before returning information to the client.
*   `select_account`

    The authorization server prompts the user to select a user account. This allows a user who has multiple accounts at the authorization server to select amongst the multiple accounts that they may have current sessions for.

If no value is specified and the user has not previously authorized access, then the user is shown a consent screen.

#### Connect with specified chain

```typescript

import { BNBChain } from '@particle-network/chains';

// connect with social type.
const userInfo = await connect({
    socialType: 'google',
    prompt: 'select_account', //optional
    chain: BNBChain,
});

```

#### Connect and authorize

When connect Auth Core, [`Sign-In with Ethereum`](https://eips.ethereum.org/EIPS/eip-4361)(EVM) or `signMessage`(Solana) will auto run.

```typescript

import { BNBChain } from '@particle-network/chains';

// connect with social type.
const userInfo = await connect({
    socialType: 'google',
    prompt: 'select_account', //optional
    chain: BNBChain,
    authorization: {
        uniq: true,  // uniq sign
        message: 'base58 string', // solana need it, evm will ignore it
    }
});
// get authorize signature
const signature = userInfo.signature;
```

### Disconnect

The user wallet local information will be deleted.

<pre class="language-typescript"><code class="lang-typescript"><strong>
</strong><strong>import { useConnect } from '@particle-network/auth-core-modal';
</strong>
const { disconnect } = useConnect();
// disconnect
await disconnect();
</code></pre>

### EVM Chains

Ecdsa Wallet will be created when connect with EVM chains.

```typescript

import { useEthereum } from '@particle-network/auth-core-modal';

const { provider,   // eip1193 provider
        address,    // evm wallet public address
        chainId,    // current chain
        chainInfo,
        switchChain,
        signMessage,
        signTypedData,
        sendTransaction,
        enable } = useEthereum();

// send transaction.
const txHash = await sendTransaction({
    to: '0xe8fc0baE43aA264064199dd494d0f6630E692e73',
    value: '1000000',
});

// sign message.
const signature = await signMessage(message);

// sign typed data
const signature = await signTypedData(typedData);

// switch chain to Ethereum Mainnet, chainId: number or prefixed hex string
await switchChain('0x1')

```

It is easy to use hook methods, you can also set `provider` to [web3.js](https://github.com/web3/web3.js) / [ethers.js](https://github.com/ethers-io/ethers.js) / [viem](https://github.com/wagmi-dev/viem) and use third-party lib api.

#### Create wallet manually

If you connect Auth Core with Solana chain, the EVM wallet will not be created, and the `address` will return `null`. You can create wallet manually.

```typescript

import { useEthereum } from '@particle-network/auth-core-modal';

const { address, enable } = useEthereum();

if (!address) {
    await enable();
}

```

### Solana

EdDsa Wallet will be created when connect with Solana.

```typescript

import { useSolana } from '@particle-network/auth-core-modal';

const { address,  // solana public address
        chainId,  // solana current chain id
        chainInfo,
        switchChain,
        signMessage,
        signTransaction,
        signAllTransactions,
        signAndSendTransaction,
        enable } = useSolana();

// send transaction.
const txHash = await signAndSendTransaction(txData);

// sign message.
const signature = await signMessage(message);

```

#### Create wallet manually

If you connect Auth Core with EVM chain, the Solana wallet will not be created, and the `address` will return `null`. You can create wallet manually.

```typescript

import { useSolana } from '@particle-network/auth-core-modal';

const { address, enable } = useSolana();

if (!address) {
    await enable();
}
```

### Auth Core Hooks

#### useConnect

Connect AuthCore and get connection status.

<pre class="language-typescript"><code class="lang-typescript"><strong>
</strong>import { useConnect } from '@particle-network/auth-core-modal';

const {
    connect,                    // connect auth core method
    disconnect,                 // disconnect auth core method
    connectionStatus,           // connection status: 'loading' | 'connecting' | 'connected' | 'disconnected'
    connected,                  // whether connected, true or false
    requestConnectCaptcha,      // request email or phone captcha method
    setSocialConnectCallback,   // set social connect callback, need to set up early
} = useConnect();

</code></pre>

#### useAuthCore

Get user info and manage modal.

```typescript

import { useAuthCore } from '@particle-network/auth-core-modal';

const {
    userInfo,                       // user info, null is returned if not connected
    needRestoreWallet,              // whether need restore wallet, if user has set master password and not input it, will return true
    openAccountAndSecurity,         // open account and security modal
    openSetMasterPassword,          // open set master password modal
    openChangeMasterPassword,       // open change master password modal
    openRestoreByMasterPassword,    // open input master password modal
    openSetPaymentPassword,         // open set payment password modal
    openChangePaymentPassword,      // open change payment password modal
    openSetSecurityAccount,         // open set security account modal
    openLinkLoginAccount,           // open link login account modal
    openWallet,                     // open wallet in iframe
    buildWalletUrl,                 // get wallet url, used for open in custom iframe
    openBuy,                        // open buy crypto token website
} = useAuthCore();
    
```

#### useCustomize

Set the styles of auth modal and wallet modal dynamically.

```typescript

import { useCustomize } from '@particle-network/auth-core-modal';

const { 
    setThemeType,       // set theme type, 'light' or 'dark'
    setCustomStyle,     // set auth modal styles
    setWalletOptions,   // set wallet modal options
    setLanguage,        // set language
    setFiatCoin,        // set fiat coin
    setERC4337          // set whether support ERC-4337
} = useCustomize();

```

#### useEthereum

EVM wallet interface, [learn more](web.md#evm-chains).

#### useSolana

Solana wallet interface, [learn more](web.md#solana-chains).

## Customize

You can tweak the Auth Core Modal to match your branding. Refer to [Customize](https://core-demo.particle.network/customize.html).

Set your custom config when init Auth Core.

```typescript

import { AuthType } from '@particle-network/auth-core';
import { AuthCoreContextProvider, PromptSettingType } from '@particle-network/auth-core-modal';
import { BNBChain, Ethereum } from '@particle-network/chains';

const App = () => {
    // replace 'xxxx' with particle project config.
    return (
        <AuthCoreContextProvider
            options={{
                projectId: 'xxxx',
                clientKey: 'xxxx',
                appId: 'xxxx',
                authTypes: [AuthType.email, AuthType.google, AuthType.apple],
                themeType: 'dark',  //light or dark
                fiatCoin: 'USD',
                language: 'en',
                customStyle: {
                    logo: 'https://xxxx', // image url
                    projectName: 'xxx',
                    modalBorderRadius: 16,
                    theme: {
                        light: {
                            textColor: '#000',
                        },
                        dark: {
                            textColor: '#fff',
                        }
                    }
                },
                wallet: {
                    visible: true,  //show wallet entrance when connect success.
                    customStyle: {
                        supportChains: [BNBChain, Ethereum],
                    }
                },
            }}
        >
            <YourApp />
        </AuthCoreContextProvider>
    );
};

```

For more customization options in Auth Modal, please see [Customize](https://core-demo.particle.network/customize.html).

### Wallet Modal Customize

Refer to [Wallet Service](../../wallet-service/sdks/web.md#custom-particle-wallet-style).

## Live Demo

[Auth Core Demo](https://core-demo.particle.network/)

[Source Code](https://github.com/Particle-Network/particle-web-auth-core)



