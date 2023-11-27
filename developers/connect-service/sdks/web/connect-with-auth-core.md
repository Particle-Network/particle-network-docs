# Connect With Auth Core

## Demo

Live Demo: [https://core-demo.particle.network/connect.html](https://core-demo.particle.network/connect.html)

Github: [https://github.com/Particle-Network/particle-web-auth-core](https://github.com/Particle-Network/particle-web-auth-core)

<mark style="color:red;">**It is strongly discouraged to use private key or mnemonic import/generate function, if you use it, you need to secure the data yourself, Particle's SDK has no relationship with the imported/generated mnemonic or private key.**</mark>

## Quick Start

### Step 1: Install Connect SDK.

<pre class="language-bash"><code class="lang-bash">
# react connect kit
<strong>yarn add @particle-network/connectkit
</strong><strong>
</strong></code></pre>

### Step 2: Init Connect SDK.

<pre class="language-typescript"><code class="lang-typescript"><strong>import { ModalProvider } from '@particle-network/connectkit';
</strong>import { Ethereum, EthereumGoerli } from '@particle-network/chains';
import { evmWallets } from '@particle-network/connectors';

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
                wallet: {    //optional: particle wallet config
                    visible: true, //display wallet button when connect particle success.
                    supportChains:[
                        Ethereum,
                        EthereumGoerli
                    ],
                    customStyle: {}, //optional: custom wallet style
                },
                promptSettingConfig: { //optional: particle security account config
                    //prompt set payment password. 0: None, 1: Once(default), 2: Always
                    promptPaymentPasswordSettingWhenSign: 1,
                    //prompt set master password. 0: None(default), 1: Once, 2: Always
                    promptMasterPasswordSettingWhenLogin: 1
                },
                connectors: evmWallets({ 
                    projectId: 'walletconnect projectId', //replace with walletconnect projectId
                    showQrModal: false
                 }),
            }}
            theme={'light'}
            language={'en'}   //optional：localize, default en
            walletSort={['Particle Auth', 'Wallet']} //optional：walelt order
        >
            &#x3C;App />
        &#x3C;/ModalProvider>
    &#x3C;/React.StrictMode>
);
</code></pre>

### Step3: Add the connect button

Then, in your app, import and render the `ConnectButton` component.

```typescript
import '@particle-network/connectkit/dist/index.css';
import { ConnectButton } from '@particle-network/connectkit';

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
Have a problem? you can refer to this [FAQ](../../../faq.md#web-sdk-integration-problems).
{% endhint %}

### Check User Is Connected

If user connect wallet success, automatic connect next time.

```typescript
import { useAccount } from '@particle-network/connectkit';

//use this in react component.
const account = useAccount();
if (account) {
    // connect wallet success
}
```

### Custom Wallet Style

When "visible" true, you can custom wallet style by set "customStyle" config, refer to [Custom Wallet Style](../../../wallet-service/sdks/web.md#custom-particle-wallet-style)

### Get User Info

If you connect with Particle Network, you can get user info by below interface.

<pre class="language-typescript"><code class="lang-typescript">import { useAuthCore } from '@particle-network/auth-core-modal';
import { getUserInfo } from '@particle-network/auth-core';

<strong>//use this in react component.
</strong>const { useInfo } = useAuthCore();

//or
const userInfo = getUserInfo();
</code></pre>

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

## Auth Core

If you connect wallet with auth core (email/phone/google/apple... etc.), you can also use [Auth Core Modal Hooks](../../../auth-service/core/web.md#auth-core-hooks).
