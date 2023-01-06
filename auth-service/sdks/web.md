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

## Getting Started[​](https://docs.walletconnect.com/1.0/#getting-started) <a href="#getting-started" id="getting-started"></a>

### Step 1: Include Particle Network SDK Script

Download Particle Network SDK to your project via Yarn

```bash
yarn add @particle-network/auth

//if you need support evm-chains
yarn add @particle-network/provider

//if you need support solana chain
yarn add @particle-network/solana-wallet

```

```html
browser

<script src="https://static.particle.network/sdks/web/auth/0.8.0/auth.min.js"></script>
<!-- Optional: Add EVM Chains suport -->
<script src="https://static.particle.network/sdks/web/provider/0.8.0/provider.min.js"></script>
<!-- Optional: Add Solana Chain suport -->
<script src="https://static.particle.network/sdks/web/solana-wallet/0.8.0/solana-wallet.min.js"></script>
```

### Step 2: Setup Developer API Key

Before you can add Auth Service to your app, you need to create a Particle project to connect to your app. Visit [Particle Dashboard](broken-reference) to learn more about Particle projects and apps.

[👉 Sign up/log in and create your project now](https://dashboard.particle.network/#/login)

<pre class="language-typescript"><code class="lang-typescript">import { ParticleNetwork, WalletEntryPosition } from "@particle-network/auth";
import { ParticleProvider } from "@particle-network/provider";
import { SolanaWallet } from "@particle-network/solana-wallet";
import Web3 from "web3";

const pn = new ParticleNetwork({
  projectId: "xx",
  clientKey: "xx",
  appId: "xx",
  chainName: "Ethereum", //optional: current chain name, default Ethereum.
  chainId: 1, //optional: current chain id, default 1.
  wallet: {   //optional: by default, the wallet entry is displayed in the bottom right corner of the webpage.
    displayWalletEntry: true,  //show wallet entry when connect particle.
    defaultWalletEntryPosition: WalletEntryPosition.BR, //wallet entry position
    supportChains: [{ id: 1, name: "Ethereum"}, { id: 5, name: "Ethereum"}], // optional: web wallet support chains.
<strong>    customStyle: {}, //optional: custom wallet style
</strong>  }
});

const particleProvider = new ParticleProvider(pn.auth);
//if you need support solana chain
const solanaWallet = new SolanaWallet(pn.auth);

//if you use web3.js
window.web3 = new Web3(particleProvider);
window.web3.currentProvider.isParticleNetwork // => true

//if you use ethers.js
import { ethers } from "ethers";
const ethersProvider = new ethers.providers.Web3Provider(providerProvider, "any");
const ethersSigner = ethersProvider.getSigner();
</code></pre>

Your first Particle Network dApp! 🎉 You can implement web3 functionalities just like how you would normally with MetaMask.

{% hint style="info" %}
Have a problem? you can refer to this [FAQ](../../resouces/faq.md#web-sdk-integration-problems).
{% endhint %}

### Security Account

```typescript
import { ParticleNetwork } from "@particle-network/auth";

// open security account settings
const pn = new ParticleNetwork({...});
pn.auth.accountSecurity().then((error) => {
    if (error.code === 4011) {
        //ignore window close
    } else if (error.code === 10005) {
        //invalid token
    } else if (error.code === 8005) {
        //user not login
    }
});
```

### [Custom Wallet Style](../../wallet-service/sdks/web.md#custom-particle-wallet-style)

### Tips

If you use `window.ethereum` to call RPC, The following things need to be noted:

#### Only use the Particle

Use only the Particle wallet and block other plugin wallets. For example, you can directly replace the global variable window.ethereum injected by the Metamask plugin with particleProvider:&#x20;

```typescript
window.ethereum = particleProvider;
```

#### Co-exists with other wallets

The Particle wallet co-exists with other plug-in wallets. You can create a new Provider object when switching wallets, avoiding contamination of Particle or Ethereum with assignment operations：

```typescript
let provider = Object.create(particleProvider);
const ParticleWallet = document.getElementById("ParticleWallet");
const MetaMaskWallet = document.getElementById("MetaMaskWallet");
ParticleWallet.onclick = async () => {
  provider = Object.create(particleProvider);
  ethersProvider = new ethers.providers.Web3Provider(provider, "any");
};
MetaMaskWallet.onclick = async () => {
  if (window.ethereum) {
    provider = Object.create(window.ethereum);
  }
  ethersProvider = new ethers.providers.Web3Provider(provider, "any");
};
const accounts = await provider.request({
  method: "eth_requestAccounts",
});
```

## Web3 Integration

### Network Configuration

```typescript
import { ParticleNetwork } from "@particle-network/auth";

//ethereum Kovan test net
const pn = new ParticleNetwork({
  projectId: "xx",
  clientKey: "xx",
  appId: "xx",
  chainName: "Ethereum",
  chainId: 1, //mainnet
});

//bsc testnet
const pn = new ParticleNetwork({
  projectId: "xx",
  clientKey: "xx",
  appId: "xx",
  chainName: "BSC",
  chainId: 56, // mainnet
});

//...
```

Particle support more [EVM Chains](../../node-service/evm-chains-api/#structure).

### [👉 ChainId And ChainName Configs](../../node-service/evm-chains-api/#structure)

### Get User Account

In order for web3 to work and grab the end-users' Ethereum wallet addresses, the users have to login first (similar to unlocking account in MetaMask). You can simply trigger the login for users with the web3 function call below.

```typescript
import { ParticleNetwork } from "@particle-network/auth";
import Web3 from "web3";

const pn = new ParticleNetwork({...});

// Request user login if needed, returns current user info
pn.auth.login()

// optional: custom login params.
// support auth types: email,phone,facebook,google,apple,discord,github,twitch,microsoft,linkedin
pn.auth.login({
    // when set social login auth type, will open thirdparty auth page directly.
    preferredAuthType?: AuthType,
    // when set email/phone account and preferredAuthType is email or phone, 
    // Particle Auth will enter directly input verification code page.
    // when set JWT value and preferredAuthType is jwt, Particle Auth will auto login.
    account?: string,
    supportAuthTypes?: string, //need support social login types, split with ','. default value 'all'.
    loginFormMode?: boolean, // login form mode will hide others ui except login form. only support supportAuthTypes equals email or phone.
    hideLoading?: boolean, //hide particle loading when use jwt authorization.
    socialLoginPrompt?: string, //social login prompt.  none | consent | select_account
  })
```

Login with Phone and input verification code directly.

```typescript
pn.auth.login({
    preferredAuthType: 'phone',
    account: '+14155552671', //phone number must use E.164
  });
```

Login with Social Account.

```typescript
pn.auth.login({
    preferredAuthType: 'google', //support facebook,google,apple,discord,github,twitch,microsoft,linkedin etc.
    socialLoginPrompt: 'none', //default vaue is none
  })
```

Login with JWT

```typescript
pn.auth.login({
    preferredAuthType: 'jwt',
    account: 'JWT Value',
    hideLoading: true,   //optional: hide particle loading when login.
  })
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

A modal will open to ask users to sign up for an account or login with their mobile phone number/email or social account.

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
  displayWallet: true, // display wallet entrance when send transaction
});

```

#### Switch Chain Info

```typescript
import { ParticleNetwork } from "@particle-network/auth";

const pn = new ParticleNetwork({...});
// you can set chain info when new ParticleNetwork, or call setChainInfo
pn.switchChain({
    name: "Polygon",
    id: 137,
})
```

## Open Particle Web Wallet

When connect particle auth success, you can open particle wallet by below interface.

```typescript
import { ParticleNetwork } from "@particle-network/auth";

const pn = new ParticleNetwork({...});

// Need check login state when open wallet.
// To set target and features for custom window style, same as window.open().
pn.openWallet(target?: string, features?: string)


//open wallet in iframe.
const url = pn.buildWalletUrl({
    //optional: left top menu style, close or fullscreen
    //"fullscreen": wallet will be fullscreen when user click.
    //"close": developer need handle click event
    topMenuType: "close"   
});

const iframe = document.createElement("iframe");
iframe.src = url;
//if topMenuType is "close"
window.addEventListener("message", (event) => {
    if (event.data === "PARTICLE_WALLET_CLOSE_IFRAME") {
        //close click event
    }
})

```

## Open Crypto Token Buy

When initializing the Particle is complete, you can open the Buy Tokens page.

```typescript
import { ParticleNetwork } from "@particle-network/auth";

const pn = new ParticleNetwork({...});
// open buy
// To set target and features for custom window style, same as window.open().
pn.openBuy(options?: OpenBuyOptions, target?: string, features?: string)

```

&#x20;You can customize the open buy page by setting `options` parameters.

|      name     |                                     description                                     |  type  | required |
| :-----------: | :---------------------------------------------------------------------------------: | :----: | :------: |
|    network    |             \[Solana, Ethereum, Binance Smart Chain, Avalanche, Polygon]            | string |   False  |
|    fiatCoin   |                                    Fiat currency                                    | string |   False  |
|   cryptoCoin  |                                   crypto currency                                   | string |   False  |
|    fiatAmt    |  The amount of fiat currency that the user wants to spend.It's just for Buy Crypto  | number |   False  |
|    cryptAmt   | The amount of crypto currency that the user wants to sell.It's just for Sell Crypto | number |   False  |
|  fixFiatCoin  |                       Prevent user from changing fiat currency                      |  bool  |   False  |
| fixCryptoCoin |                    Prevent user from changing fiat cryptocurrency                   |  bool  |   False  |
|   fixFiatAmt  |                  Prevent user from changing amount of fiat currency                 |  bool  |   False  |
| walletAddress |                   Wallet address for the predefined cryptocurrency                  | string |   False  |

{% hint style="info" %}
If Particle not connected. network and walletAddress are requried.
{% endhint %}

## EVM Web3Modal Integration

If you use [web3modal](https://github.com/WalletConnect/web3modal) connect wallet, you can use custom provider to add particle auth.

[👉 Sample](https://web-demo.particle.network/)

[👉 Source Code](https://github.com/Particle-Network/particle-web-demo/blob/master/src/web3Modal.ts)



## EVM RainbowKit Integration

If you use [RainbowKit](https://github.com/rainbow-me/rainbowkit) connect wallet, you can add particle auth quickly.

<figure><img src="https://static.particle.network/docs-images/rainbowkit-particle-ext.png" alt=""><figcaption></figcaption></figure>

```bash
//install particle rainbowkit extension.
yarn add @particle-network/rainbowkit-ext
```

Init Particle.

```typescript
// replace config 'xxx'
new ParticleNetwork({
    appId: 'xxx',
    clientKey: 'xxx',
    projectId: 'xxx',
});
```

Add Particle to RainbowKit wallet list.

```typescript
// add particle in group Popular
const popularWallets = {
  groupName: "Popular",
  wallets: [
    particleWallet({ chains, authType: "google" }),
    particleWallet({ chains, authType: "facebook" }),
    particleWallet({ chains, authType: "apple" }),
    particleWallet({ chains }),
    injectedWallet({ chains }),
    rainbowWallet({ chains }),
    coinbaseWallet({ appName: "RainbowKit demo", chains }),
    metaMaskWallet({ chains }),
    walletConnectWallet({ chains }),
  ],
};
```

[👉 Sample](https://web-demo.particle.network/)

👉 [Source Code](https://github.com/Particle-Network/particle-web-demo/blob/master/src/rainbowkit/rainbowkit-demo.ts)

## EVM WalletConnect V1 Integration

### Introduction <a href="#introduction" id="introduction"></a>

[WalletConnect](https://docs.walletconnect.com/1.0/) is an open protocol to communicate securely between Wallets and Dapps (Web3 Apps). If you integration Particle Auth, you can connect with other Dapps with WalletConnect protocol.

### Getting Started[​](https://docs.walletconnect.com/1.0/#getting-started) <a href="#getting-started" id="getting-started"></a>

Download SDK to your project via Yarn.

```powershell
yarn add @particle-network/walletconnect @particle-network/auth
```

Init SDK.

```typescript
import { particleWalletConnect, IWalletConnectSession, SwitchChainEvent } from '@particle-network/walletconnect';

particleWalletConnect.init(evmProvider);
particleWalletConnect.hub.on(SwitchChainEvent.request, (options) => {
    //TODO: listen switch chain
    
    //approve switch chain
    particleWalletConnect.approveSwitchChain(options.session.peerId);
    
    //or reject switch chain
    particleWalletConnect.rejectSwitchChain(options.session.peerId);
})
```

Connect to Dapps.

```typescript
// connect dapp with uri, you can get uri from qrcode or clipboard.
const connectDapps = async (uri: string) => {
    particleWalletConnect.hub.once(SessionRequestEvent.request, (peerInfo) => {
      //TODO: receive dapps session request. show confirm modal for user.
      
      //approve dapps session request.
      particleWalletConnect.approveSessionRequest(walletconnectData.peerInfo.peerId);
      //or reject dapps session request.
      particleWalletConnect.rejectSessionRequest(walletconnectData.peerInfo.peerId);
    });
    particleWalletConnect.hub.once(SessionRequestEvent.connected, (session: IWalletConnectSession) => {
      //TODO: dapps connect success.
    });
    particleWalletConnect.hub.once(SessionRequestEvent.error, ({ session, error }: { session: IWalletConnectSession; error: Error }) => {
      //TODO: dapps connect error.
    });
    await particleWalletConnect.newSession(uri);
};
```

Other APIs

```typescript
// get all connected sessions
particleWalletConnect.getSessions();

//kill session
await particleWalletConnect.killSession(peerId);

// check uri is connected
particleWalletConnect.isSessionConnected(uri);

// check string is walletconnect uri
particleWalletConnect.isValidUri(txt);
```

### Demo

[Particle Wallet](https://wallet.particle.network/) has already supported this protocol, you can use it scan qrcode connect to Dapps.

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
