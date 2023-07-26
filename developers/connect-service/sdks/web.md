---
description: A simple way to connect wallet to DApp, support EVM and Solana chains.
---

# Web

## Demo

Live Demo: [https://web-demo.particle.network/](https://web-demo.particle.network/)

Github: [https://github.com/Particle-Network/particle-web-demo](https://github.com/Particle-Network/particle-web-demo)

<mark style="color:red;">**It is strongly discouraged to use private key or mnemonic import/generate function, if you use it, you need to secure the data yourself, Particle's SDK has no relationship with the imported/generated mnemonic or private key.**</mark>

## Quick Start

### Step 1: Install Connect SDK.

<pre class="language-bash"><code class="lang-bash"># react connect kit
<strong>yarn add @particle-network/connect-react-ui
</strong>
# optional: if you only use connect interface, also support vue
yarn add @particle-network/connect

</code></pre>

### Step 2: Init Connect SDK.

<pre class="language-typescript"><code class="lang-typescript"><strong>import { ModalProvider } from '@particle-network/connect-react-ui';
</strong>import { WalletEntryPosition } from '@particle-network/auth';
import { Ethereum, EthereumGoerli } from '@particle-network/chains';
import { evmWallets } from '@particle-network/connect';

// use react kit
const root = ReactDOM.createRoot(document.getElementById('root') as HTMLElement);
root.render(
    &#x3C;React.StrictMode>
        &#x3C;ModalProvider
            options={{
                projectId: 'replace with your projectId',
                clientKey: 'replace with your clientKey',
                appId: 'replace with your appId',
                chains: [
                    Ethereum,
                    EthereumGoerli
                ],
                particleWalletEntry: {    //optional: particle wallet config
                    displayWalletEntry: true, //display wallet button when connect particle success.
                    defaultWalletEntryPosition: WalletEntryPosition.BR,
                    supportChains:[
                        Ethereum,
                        EthereumGoerli
                    ],
                    customStyle: {}, //optional: custom wallet style
                },
                securityAccount: { //optional: particle security account config
                    //prompt set payment password. 0: None, 1: Once(default), 2: Always
                    promptSettingWhenSign: 1,
                    //prompt set master password. 0: None(default), 1: Once, 2: Always
                    promptMasterPasswordSettingWhenLogin: 1
                },
                wallets: evmWallets({ 
                    projectId: 'walletconnect projectId', //replace with walletconnect projectId
                    showQrModal: false
                 }),
            }}
            theme={'auto'}
            language={'en'}   //optional：localize, default en
            walletSort={['Particle Auth', 'Wallet']} //optional：walelt order
            particleAuthSort={[    //optional：display particle auth items and order
                'email',
                'phone',
                'google',
                'apple',
                'facebook'
            ]}
        >
            &#x3C;App />
        &#x3C;/ModalProvider>
    &#x3C;/React.StrictMode>
);
</code></pre>

### Step3: Add the connect button

Then, in your app, import and render the `ConnectButton` component.

```typescript
import '@particle-network/connect-react-ui/dist/index.css';
import { ConnectButton } from '@particle-network/connect-react-ui';

export const App = () => {
  return <ConnectButton />;
};

// if you want to custom connet button, you can use ConnectButton.Custom.
export const App = () => {
  return (
        <ConnectButton.Custom>
            {({ account, chain, openAccountModal, openConnectModal, openChainModal, accountLoading }) => {
                return (
                    <div>
                        <button onClick={openConnectModal} disabled={!!account}>
                            Open Connect
                        </button>
                        <br />
                        <br />
                        <button onClick={openAccountModal} disabled={!account}>
                            Open Account
                        </button>
                        <br />
                        <br />
                        <button onClick={openChainModal} disabled={!account}>
                            Open Switch Network
                        </button>
                        <div>
                            <h3>account</h3>
                            <p>{account}</p>
                        </div>
                    </div>
                );
            }}
        </ConnectButton.Custom>
    );
};

```

{% hint style="info" %}
Have a problem? you can refer to this [FAQ](../../faq.md#web-sdk-integration-problems).
{% endhint %}

### Check User Is Connected

If user connect wallet success, automatic connect next time.

```typescript
import { useAccount } from '@particle-network/connect-react-ui';

//use this in react component.
const account = useAccount();
if (account) {
    // connect wallet success
}
```

### Custom Wallet Style

When "displayWalletEntry" true, you can custom wallet style by set "customStyle" config, refer to [Custom Wallet Style](../../wallet-service/sdks/web.md#custom-particle-wallet-style)

### Open Particle Web Wallet

If you connect with Particle Network, you can open wallet in new window.

```typescript
import { useConnectKit } from '@particle-network/connect-react-ui';

//use this in react component.
const connectKit = useConnectKit();
// To set target and features for custom window style, same as window.open().
connectKit.particle.openWallet(target?: string, features?: string);

//open wallet in custom iframe.
const url = connectKit.particle.buildWalletUrl({
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

### Open Crypto Token Buy

If you connect with Particle Network, you can open the Buy Tokens page.

```typescript
import { useConnectKit } from '@particle-network/connect-react-ui';

//use this in react component.
const connectKit = useConnectKit();
// To set target and features for custom window style, same as window.open().
connectKit.particle.openBuy(options?: OpenBuyOptions, target?: string, features?: string)
```

### Get User Info

If you connect with Particle Network, you can get user info by below interface.&#x20;

```typescript
import { useConnectKit } from '@particle-network/connect-react-ui';

//use this in react component.
const connectKit = useConnectKit();
const userInfo = connectKit.particle.getUserInfo();
```

## Connect React Hooks

### useParticleProvider

Get provider when wallet connect, you can set the provider to [web3.js](https://github.com/web3/web3.js) or [ethers.js](https://github.com/ethers-io/ethers.js/).

### useAccount

Get wallet address when wallet connect.

### useParticleConnect

Get `connect()` and `disconnect()` function for action.

### useConnectKit

Get `ParticleConnect` instance do custom action.

### useConnectModal

openConnectModal without ConnectButton.

### useConnectId

Get connected id.

### useSwitchChains

Check if you can switch chain, and get chains.

### useLanguage

Get current language and set language.

### useWalletMetas

Get registered wallet metadata.

## Connect Core

You can only use `@particle-network/connect` to custom UI.

```typescript
import { evmWallets, ParticleConnect } from '@particle-network/connect';
import { Ethereum } from '@particle-network/chains';

// init
const connectKit = new ParticleConnect({
                projectId: 'replace with your projectId',
                clientKey: 'replace with your clientKey',
                appId: 'replace with your appId',
                chains: [
                    Ethereum
                ],
                wallets: evmWallets({ qrcode: false }),
            });

//connect registered wallet. metamask, walletconnect_v2 etc.
connectKit.connect('wallet id');

//connect to particle
connectKit.connect('particle', options);

//disconnect wallet
connectKit.disconnect();

//connect last wallet
connectKit.connectToCachedProvider();

//listen provider event
connectKit.on('connect', (provider) => {});
connectKit.on('disconnect', () => {});
connectKit.on('chainChanged', (chain) => {});
connectKit.on('accountsChanged', (accounts) => {});

//switch chain
connectKit.switchChain(chain);

//get registered wallet metadata
connectKit.walletMetas();

//get evm default wallets
evmWallets();   //you can also custom wallet order

//get browser injected evm wallet 
evmInjectedWallet();

//get browser injected wallet metadata
getInjectedProvider();


```



