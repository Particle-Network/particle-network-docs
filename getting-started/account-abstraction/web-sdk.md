# Web SDK

## Installation[​](https://docs.walletconnect.com/1.0/#getting-started) <a href="#getting-started" id="getting-started"></a>

Install and import the biconomy package from the Particle Network SDK

```sh
yarn add @particle-network/biconomy
//or
npm install @particle-network/biconomy
```

## Initialize the SmartAccount

You need to create a Particle project to connect to your app, Visit [Particle Dashboard](../dashboard/) to learn more about Particle projects and apps.

And create a Biconomy Paymasters to connect AA, Visit [Biconomy Dashboard](https://dashboard.biconomy.io/) to learn more.

The provider is [EIP-1193: Ethereum Provider JavaScript API ](https://eips.ethereum.org/EIPS/eip-1193), you can use Particle Provider, MetaMask Provider or any EVM wallet provider.

```typescript
import { SmartAccount } from '@particle-network/biconomy';

//provider: eip1193
const smartAccount = new SmartAccount(provider, {
    projectId: 'particle project id',
    clientKey: 'particle client key',
    appId: 'particle app id',
    networkConfig: [
        { dappAPIKey: 'biconomy api key', chainId: 1 },
        { dappAPIKey: 'biconomy api key', chainId: 5 }
    ],
});
```

## Get Smart Account Address

Once you have connected your EOA signer and created an instance of Smart Account, the address is the public property and remains the same across all EVM chains.

<pre class="language-typescript"><code class="lang-typescript">// AA address
<strong>const address = await smartAccount.getAddress();
</strong>// EOA address
const address = await smartAccount.getOwner();
// load account more info.
const accountInfo = await smartAccount.getAccount();
</code></pre>

## Sending Gasless Transactions

Gasless sponsorships can be managed by passing relevant parameters in the Smart Account custom config.

```typescript
// replace with real transaction
const tx = {
    to,
    value,
    data,
};
const txHash = await smartAccount.sendGaslessTransaction(tx);

//batch gasless transactions
const txs = [tx1, tx2, tx3];
const txHash = await smartAccount.sendGaslessTransaction(txs);
```

## Sending User Paid Transactions

User can pay gas with ERC20 token.

<pre class="language-typescript"><code class="lang-typescript">// replace with real transaction
const tx = {
    to,
    value,
    data,
};
<strong>// get fee quotes first.
</strong>const feeQuotes = await smartAccount.getFeeQuotes(tx);
// Let the user choose to pay gas token
const feeQuote = feeQuotes[0];
const txHash = await smartAccount.sendUserPaidTransaction(tx, feeQuote);


// batch user paid transactions
const txs = [tx1, tx2];
// get fee quotes first.
const feeQuotes = await smartAccount.getFeeQuotes(txs);
// Let the user choose to pay gas token
const feeQuote = feeQuotes[0];
const txHash = await smartAccount.sendUserPaidTransaction(txs, feeQuote);

</code></pre>

## Wallet Deployment flow

Any gasless or user paid transaction will check whether the wallet is deployed or not. If the wallet is not deployed, the SDK will batch the wallet deployment with the intended transaction/batch transactions. This behaviour is handled within the SDK itself & would require no changes in your code to enable this.

### Deploy Wallet Contract Manual

```typescript
// check the constract is deployed
const isDeploy = await smartAccount.isDeployed();
if (!isDeploy) {
    const txHash = await smartAccount.deployWalletContract();
}
```

## Wrap Provider

If your app has implemented sending transactions, or use web3.js, you can wrap the EIP-1193 provider to quickly implement AA wallet.

<pre class="language-typescript"><code class="lang-typescript">import { BiconomyWrapProvider, SendTransactionMode, SendTransactionEvent } from '@particle-network/biconomy';
import Web3 from "web3";

<strong>// sendTxMode: UserPaidNative(default), Gasless, UserSelect
</strong><strong>const wrapProvider = new BiconomyWrapProvider(smartAccount, SendTransactionMode.UserPaidNative);
</strong><strong>// replace any EIP-1193 provider to wrapProvider
</strong><strong>const web3 = new Web3(wrapProvider);
</strong>//send user paid transaction
<strong>await web3.eth.sendTransaction(tx);
</strong><strong>
</strong><strong>
</strong><strong>// send gassless transaction
</strong>const wrapProvider = new BiconomyWrapProvider(smartAccount, SendTransactionMode.Gasless);
const web3 = new Web3(wrapProvider);
await web3.eth.sendTransaction(tx);
<strong>
</strong><strong>
</strong><strong>// use select pay gas token or gasless
</strong>const wrapProvider = new BiconomyWrapProvider(smartAccount, SendTransactionMode.UserSelect);
const web3 = new Web3(wrapProvider);
wrapProvider.once(SendTransactionEvent.Request, (feeQuotes) => {
    // let the user select the pay gas token
    wrapProvider.resolveSendTransaction(feeQuotes[0]);
    
    // or send gasless
     wrapProvider.resolveSendTransaction();
    
    // or reject send transaction
    wrapProvider.rejectSendTransaction({ message: 'user rejected'});
});
await web3.eth.sendTransaction(tx);
<strong>
</strong></code></pre>
