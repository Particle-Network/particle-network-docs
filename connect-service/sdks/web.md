---
description: A simple way to connect wallet to DApp, support EVM and Solana chains.
---

# Web

## Demo

Live Demo: [https://web-demo.particle.network/](https://web-demo.particle.network/)

Github: [https://github.com/Particle-Network/particle-web-demo](https://github.com/Particle-Network/particle-web-demo)



## Quick Start

### Step 1: Install Connect SDK.

<pre class="language-bash"><code class="lang-bash">// react connect kit
<strong>yarn add @particle-network/connect-react-ui
</strong>
//optional: if you only use connect interface
yarn add @particle-network/connect
</code></pre>

### Step 2: Init Connect SDK.

```typescript
import { ModalProvider } from '@particle-network/connect-react-ui';

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
                    {
                        id: 1,
                        name: 'Ethereum',
                    }
                ],
                wallets: evmWallets({ qrcode: false }),
            }}
            theme={'auto'}
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

### useWalletMetas

Get registered wallet metadata.

## Connect Core

You can only use `@particle-network/connect` to custom UI.

```typescript
import { evmWallets, evmInjectedWallet, ParticleConnect } from '@particle-network/connect';
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

//get registered wallet metadata
connectKit.walletMetas();

//get evm default wallets
evmWallets();   //you can also custom wallet order

//get browser injected evm wallet 
evmInjectedWallet();

//get browser injected wallet metadata
getInjectedProvider();


```



