# Unity

Before using, please review [the prerequisites](../../../getting-started/platform-specific-guides/unity/unity-sdk-prerequisites.md)

All unity SDKs are open source, [click here to view](https://github.com/Particle-Network/particle-unity)

### Init

It is required before call other method, you can define your app info as a metadata or user the default value, metadata is used for wallet connect.

```csharp
var metadata = new DAppMetaData("Particle Connect",
                "https://connect.particle.network/icons/512.png",
                "https://connect.particle.network");

ChainInfo chainInfo = new AvalancheChain(AvalancheChainId.Mainnet);
// Init and set default chain info.
ParticleNetwork.Init(chainInfo);
ParticleConnectInteraction.Init(chainInfo, metadata);

// The following methods are optional, used for configure your project.

// Set support chain info array. you can set a chain info array, default value is all chains.
ParticleWalletGUI.SupportChain(new []{chainInfo});
// Disable buy.
ParticleWalletGUI.EnablePay(false);
// Disable testnet if release.
ParticleWalletGUI.ShowTestNetwork(false);
// Disable wallet manage page if you only support one wallet.
ParticleWalletGUI.ShowManageWallet(false);
// Use this method to control dark mode or light mode. you can call this method with your button.
ParticleNetwork.SetInterfaceStyle(UserInterfaceStyle.DARK);

// Manage Tokens and NFTs, set show only native and your tokens, NFTs, don't show other tokens and NFTs.
ParticleWalletGUI.SetDisplayTokenAddresses(new []{"Your token address"});
ParticleWalletGUI.SetDisplayNFTContractAddresses(new []{"Your nft address"});

// Manage Tokens and NFTs, set priority tokens and NFTs. 
ParticleWalletGUI.SetPriorityTokenAddresses(new []{"Your token address"});
ParticleWalletGUI.SetPriorityNFTContractAddresses(new []{"Your nft address"});

// Control if show add button in wallet page.
ParticleWalletGUI.SetSupportAddToken(false);

// Control if support wallet connect as a wallet
ParticleWalletGUI.SupportWalletConnect(false)

// Control if show dapp browser in wallet page
ParticleWalletGUI.SupportDappBrowser(false)

// Control UI pages native currency symbol
ParticleWalletGUI.SetFiatCoin("HKD");

// Set language
ParticleWalletGUI.SetLanguage(Language.KO);

// Control if show language setting button in setting page.
ParticleWalletGUI.ShowLanguageSetting(true);

// Control if show appearance setting button in setting page.
ParticleWalletGUI.ShowAppearanceSetting(true);
```

### Navigator Wallet

```csharp
public void NavigatorWallet()
{
    // If you want to navigator wallet token, set display to 0
    // If you want to navigator wallet NFT, set display to 1
    int display = 0;
    ParticleWalletGUI.NavigatorWallet(display);
}
```

### Navigator Token Receive

```csharp
public void NavigatorTokenReceive()
{
    // If you want to test solana, your should replace under value with solana test account.
    // Navigator to token receive page
    // This is ChainLink Token in Ethereum Gnerli
    string tokenAddress = TestAccount.EVM.TokenContractAddress;
    ParticleWalletGUI.NavigatorTokenReceive(tokenAddress);
}
```

### Navigator Token Send

```csharp
public void NavigatorTokenSend()
{
    // If you want to test solana, your should replace under value with solana test account.
    // This is ChainLink Token in Ethereum Gnerli
    string tokenAddress = TestAccount.EVM.TokenContractAddress;
    // Another receiver address
    string toAddress = TestAccount.EVM.ReceiverAddress;
    // Send amount
    string amount = "1000000000";
    ParticleWalletGUI.NavigatorTokenSend(tokenAddress, toAddress, amount);
}
```

### Navigator Token Transaction Records

```csharp
public void NavigatorTokenTransactionRecords()
{
    // If you want to test solana, your should replace under value with solana test account.
    // This is ChainLink Token in Ethereum Gnerli
    string tokenAddress = TestAccount.EVM.TokenContractAddress;
    ParticleWalletGUI.NavigatorTokenTransactionRecords(tokenAddress);
}
```

### Navigator NFT Send

```csharp
public void NavigatorNFTSend()
{
    // If you want to test solana, your should replace under value with solana test account.
    // This is a test NFT
    string mint = TestAccount.EVM.NFTContractAddress;
    string tokenId = TestAccount.EVM.NFTTokenId;
    string receiveAddress = "";
    ParticleWalletGUI.NavigatorNFTSend(mint, tokenId, receiveAddress);
}
```

### Navigator NFT Details

```csharp
public void NavigatorNFTDetails()
{
    // If you want to test solana, your should replace under value with solana test account.
    // This is a test NFT
    string mint = TestAccount.EVM.NFTContractAddress;
    string tokenId = TestAccount.EVM.NFTTokenId;
    ParticleWalletGUI.NavigatorNFTDetails(mint, tokenId);
}
```

### Navigator Buy Crypto

```csharp
public void NavigatorBuyCrypto()
{
    // buy crypto with parameters
    BuyCryptoConfig config = new BuyCryptoConfig(TestAccount.EVM.PublicAddress,
                OpenBuyNetwork.BinanceSmartChain, "BNB", "USD", 100);
    ParticleWalletGUI.NavigatorBuyCrypto(config);

    // also support buy crypto without parameters
    ParticleWalletGUI.NavigatorBuyCrypto();
    
}
```

### Navigator Login List

```csharp
public async void NavigatorLoginList()
{
    var nativeResultData = await ParticleWalletGUI.Instance.NavigatorLoginList();
    if (nativeResultData.isSuccess)
    {
        Debug.Log(nativeResultData.data);
    }
    else
    {
        var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
        Debug.Log(errorData);
    }
}
```

### Navigator Swap

```csharp
public void NavigatorSwap()
{
    ParticleWalletGUI.NavigatorSwap();
}
```

### Switch Wallet

You'd better call this method to sync wallet with GUI, before open any GUI page.

For example, user connects more than one wallet, then open wallet page, call this method to make sure open this right wallet in GUI pages.

```csharp
public async void SwitchWallet()
{
    var walletType = WalletType.MetaMask;
    var publicAddress = "";
    var nativeResultData = await ParticleWalletGUI.Instance.SwitchWallet(walletType, publicAddress);
    
    Debug.Log(nativeResultData.data);

    if (nativeResultData.isSuccess)
    {
        ShowToast($"{MethodBase.GetCurrentMethod()?.Name} Success:{nativeResultData.data}");
        Debug.Log(nativeResultData.data);
    }
    else
    {
        ShowToast($"{MethodBase.GetCurrentMethod()?.Name} Failed:{nativeResultData.data}");
        var errorData = JsonConvert.DeserializeObject<NativeErrorData>(nativeResultData.data);
        Debug.Log(errorData);
    }
}
```

### Set support chain infos, default value is support all chain infos.

```csharp
ChainInfo avalanche = new AvalancheChain(AvalancheChainId.Mainnet);
ChainInfo ethereum = new EthereumChain(EthereumChainId.Mainnet);
ChainInfo bsc = new BSCChain(BscChainId.Mainnet);
ParticleWalletGUI.SupportChain(new []{avalanche, bsc, ethereum});
```

### Set enable or disable swap feature, default value is true.

```csharp
ParticleWalletGUI.EnableSwap(true);
```

### Set enable or disable buy crypto feature, default value is true.

```csharp
ParticleWalletGUI.EnablePay(true);
```

### Set show or hidden test network in switch network page, default value is false.

```csharp
ParticleWalletGUI.ShowTestNetwork(false);
```

### Set show or hidden wallet manage page, default value is true.

```csharp
ParticleWalletGUI.ShowManageWallet(false);
```

### Set user interface style, default value is follow system.

```csharp
ParticleNetwork.SetInterfaceStyle(UserInterfaceStyle.DARK);
```

### Set language, default value is follow system.

```csharp
ParticleWalletGUI.SetLanguage(Language.EN);
```

### Get swap enabled **** state ****&#x20;

```csharp
public void GetSwapEnableState()
{
    var result = ParticleWalletGUI.GetEnableSwap();
    Debug.Log($"Swap enable state = {result}");
}
```

### Get buy crypto enabled state

```csharp
public void GetBuyCryptoEnableState()
{
    var result = ParticleWalletGUI.GetEnablePay();
    Debug.Log($"Buy crypto enable state = {result}");
}
```

