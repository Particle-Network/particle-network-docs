# Unreal

### Resources <a href="#prerequisites" id="prerequisites"></a>

* [Particle UE SDK Github](https://github.com/Particle-Network/particle-unreal)

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Unreal Engine 5&#x20;

### Installation

Install Particle SDK to local UE5 project

* Open your project root directory which `[ProjectName].uproject` locate at
* Create a directory, and set its name to `Plugins` . If `Plugins` already exists, skip this step.
* Copy directory `Plugins/ParticleSDK` of Particle SDK to `Plugins` directory.

Open UE5 Editor, navigator to `Menu → Edit → Plugins` , check the option to enable ParticleSDK.

### Auth Service

Try our [demo](https://github.com/Particle-Network/particle-unreal) for more use cases.

### Initialize&#x20;

initialize ParticleSDK create a `Init` function in the Widget Blueprint, and then complete its parameters.

* `Default Browser Widget`: it can be set to `W_ExecuteWebBrowser` of Particle SDK. If you want to use your custom WebBrowser Blueprint, it must follow the structure of `W_ExecuteWebBrowser`
* `In Config`: Json string, required project id, client key and appId, could have more parameters, for more details, click [here](web.md#set-auth-theme)
* `In Theme`: Json string, could be empty, it works for custom web wallet ui, for more details, click [here](web.md#set-auth-theme)
* `In Language`: support en-US, zh-CN, zh-TW, ja-JP, ko-KR
* `In Chain Name / In Chain Id`: support EVM chains and Solana, take a look [ChainId And ChainName Configs](../../node-service/evm-chains-api/#structure)

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption><p>Init</p></figcaption></figure>

### Login

create a `Login` function in the Widget Blueprint, and then complete its parameters.

* `Preferred Auth Type`: optional, you can pass "phone" / "email" / "jwt" , or empty
* `Account`: optional, you can pass phone number (E.164) / email address / json web token, or empty.
* `OnLoginEvent`: Json string, When user login succeed, user info will be returned as event data，including user uuid, token, wallet info and so on, you should persistent the user info data by yourself.

<figure><img src="../../../.gitbook/assets/image (8) (1).png" alt=""><figcaption><p>Login </p></figcaption></figure>

### Sign message

create a `SignMessage` function in the Widget Blueprint, and then complete its parameters.

1. `Message`: the message to be signed by Particle Auth Service. in evm, request utf8 string, like "hello world", in solana, request base58 string.&#x20;
2. `OnSignMessageEvent`: Json string, when user confirmed signing the message in Particle Auth Service, a result will be return as event data, it should contain error or signature.

<figure><img src="../../../.gitbook/assets/image (3) (1) (3).png" alt=""><figcaption><p>Sign message</p></figcaption></figure>

### Sign and send transaction

create a `SignAndSendTransaction` function in the Widget Blueprint, and then complete its parameters.

1. `Transaction`: the transaction to be signed by Particle Auth Service. in evm, request utf8 json string, in solana, request base58 string.&#x20;
2. `MakeEvmTransaction`: is a helper method to create a evm transaction, also you can create by yourself, then serilized to json string.
3. `OnSignAndSendTransactionEvent`: Json string, when user confirmed signing the transaction in Particle Auth Service, a result will be return as event data, it should contain error or signature.

<figure><img src="../../../.gitbook/assets/image (2) (3).png" alt=""><figcaption><p>Sign and send transaction</p></figcaption></figure>

### Sign typed data

only support evm, create a `SignTypedData` function in the Widget Blueprint, and then complete its parameters.

1. `Message`: the message to be signed by Particle Auth Service. in evm, request utf8 json string
2. `Version`: we support v1, v3, v4, default is v4.
3. `OnSignTypedDataEvent`: Json string, when user confirmed signing the message in Particle Auth Service, a result will be return as event data, it should contain error or signature.

<figure><img src="../../../.gitbook/assets/image (4) (1).png" alt=""><figcaption><p>Sign typed data</p></figcaption></figure>

### Sign transaction

only support solana, create a `SignTransaction` function in the Widget Blueprint, and then complete its parameters.

1. `Transaction`: the transaction to be signed by Particle Auth Service. request base58 string
2. `OnSignTransactionEvent`: Json string, when user confirmed signing the message in Particle Auth Service, a result will be return as event data, it should contain error or signature.

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Sign transaction</p></figcaption></figure>

### Sign all transactions

only support solana, create a `SignAllTransactions` function in the Widget Blueprint, and then complete its parameters.

1. `Transactions`: the transactions to be signed by Particle Auth Service. request base58 string array.
2. `OnSignAllTransactionsEvent`: Json string, when user confirmed signing the message in Particle Auth Service, a result will be return as event data, it should contain error or signature.

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption><p>Sign all transaction</p></figcaption></figure>
