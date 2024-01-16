---
description: First Account Abstraction Protocol on Bitcoin
---

# Web

## Installation

Get up and running with BTC Connect.

```bash

npm install @particle-network/btc-connectkit
// or
yarn add @particle-network/btc-connectkit

```

{% hint style="info" %}
Note: BTC Connect is a [React](https://reactjs.org/) library.
{% endhint %}

## Configure

Register your app on the [Dashboard](https://dashboard.particle.network/) and get the  `projectId`, `clientKey`, `appId.` Then replace `xxxx` below with your project configuration.

Create  a `ConnectProvider` react component, add `connectors` and project options.

<pre class="language-typescript"><code class="lang-typescript"><strong>'use client';
</strong>
import {
  ConnectProvider as BTCConnectProvider,
  OKXConnector,
  UnisatConnector,
  XverseConnector,
} from '@particle-network/btc-connectkit';

export default function ConnectProvider({ children }: { children: React.ReactNode }) {
  return (
    &#x3C;BTCConnectProvider
      options={{
        projectId: 'xxxx',
        clientKey: 'xxxx',
        appId: 'xxxx',
        aaOptions: {
          accountContracts: {
            BTC: [
              {
                chainIds: [686868, 28206, 80001],
                version: '1.0.0',
              },
            ],
          },
        },
        walletOptions: { // optional: wallet options
          visible: true,
        }
      }}
      connectors={[new UnisatConnector(), new OKXConnector(), new XverseConnector()]}
    >
      {children}
    &#x3C;/BTCConnectProvider>
  );
}
</code></pre>

Then wrap your application with `ConnectProvider`.

```typescript
const App = () => {
  return (
    <ConnectProvider>
        {/* Your App */}
    </ConnectProvider>
  );
};
```

## Usage

BTC Connect is implemented based on React,  You can use custom hooks to connect to the wallet and send transactions.

### Connect Wallet

```typescript
import { useConnectModal } from '@particle-network/btc-connectkit';

const { openConnectModal, disconnect } = useConnectModal();

// open connect modal
const onOpenConnectModal = () => {
    openConnectModal();
}

// disconnect wallet
const onDisconnect = () => {
    disconnect();
}
```

### Get Accounts

You can get BTC accounts and EVM smart accounts.

```typescript
import { useAccounts } from '@particle-network/btc-connectkit';

const { accounts } = useAccounts(); // btc accounts.

const { evmAccount } = useETHProvider(); // evm smart accounts.

```

### BTC Provider

After connect wallet,  you can use these hook functions interact with blockchain.

<pre class="language-typescript"><code class="lang-typescript"><strong>import { useBTCProvider } from '@particle-network/btc-connectkit';
</strong>
const { provider, getNetwork, switchNetwork, signMessage, getPublicKey, sendBitcoin } = useBTCProvider();

// switch network to testnet
await switchNetwork('testnet')

// get current network
const network = await getNetwork();

// sign message
const signature = await signMessage('Hello Particle!');

// get public key
const pubKey = await getPublicKey();

// send bitcoin
const txId = await sendBitcoin(toAddress, satoshis, options);

// and you can use provider to access other methods.
provider.sendInscription(...)

</code></pre>

### EVM Provider

EVM Smart Account auto generate when connect wallet, you can use BTC wallet signature to send EVM transaction.

```typescript
import { useBTCProvider } from '@particle-network/btc-connectkit';

const { evmAccount, smartAccount, chainId, switchChain } = useETHProvider();

// current evm chain id.
const id = chainId;

// switch evm chain
await switchChain(28206);


// send aa transaction.
const tx = {
  to: '0xe8fc0baE43aA2640.......d0f6630E692e73',
  value: '100000000000',
  data: '0x',
};
const feeQuotes = await smartAccount.getFeeQuotes(tx);
const { userOp, userOpHash } = feeQuotes.verifyingPaymasterNative;
const hash = await smartAccount.sendUserOperation({ userOp, userOpHash });
```

For more `SmartAccount` usage, refer to [Account Abstraction](../account-abstraction/).

## Preview

* [Online Demo](https://btc-connect-demo.particle.network/)
* [Source Code](https://github.com/Particle-Network/particle-btc-connect)

