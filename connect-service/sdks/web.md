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
<strong>yarn add @particle-network/connect-react-ui @particle-network/auth @particle-network/provider @particle-network/solana-wallet @walletconnect/ethereum-provider@1.8.0
</strong>
# optional: if you only use connect interface
yarn add @particle-network/connect @particle-network/auth @particle-network/provider @particle-network/solana-wallet @walletconnect/ethereum-provider@1.8.0

# if not support solana, you can add solana-wallet to devDependencies
yarn add @particle-network/solana-wallet -D

</code></pre>

### Step 2: Init Connect SDK.

```typescript
import { ModalProvider, Ethereum, EthereumGoerli, evmWallets } from '@particle-network/connect-react-ui';
import { WalletEntryPosition } from '@particle-network/auth';

// use react kit
const root = ReactDOM.createRoot(document.getElementById('root') as HTMLElement);
root.render(
    <React.StrictMode>
        <ModalProvider
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
                wallets: evmWallets({ qrcode: false }),
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
            <App />
        </ModalProvider>
    </React.StrictMode>
);
```

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
Have a problem? you can refer to this [FAQ](../../resouces/faq.md#web-sdk-integration-problems).
{% endhint %}

### [Custom Wallet Style](../../wallet-service/sdks/web.md#custom-particle-wallet-style)

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
// init
cost connectKit = new ParticleConnect({
                projectId: 'replace with your projectId',
                clientKey: 'replace with your clientKey',
                appId: 'replace with your appId',
                chains: [
                    {
                        id: 1,
                        name: 'Ethereum',
                    }
                ],
                wallets: evmWallets({ qrcode: false }),
            });

//connect registered wallet
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



