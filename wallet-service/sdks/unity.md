# Unity

Before using, please review [the prerequisites](../../dashboard/unity-sdk-prerequisites.md)

All unity sdk are open source, [click here to view](https://github.com/Particle-Network/particle-unity)



### Init

```csharp
public void Init()
{
    var metadata = DAppMetadata.Create("Particle Connect",
        "https://connect.particle.network/icons/512.png",
        "https://connect.particle.network");

    ParticleNetwork.Init(_chainInfo);
    ParticleConnectInteraction.Init(_chainInfo, metadata);
}
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

### Navigator Pay

```csharp
public void NavigatorPay()
{
    ParticleWalletGUI.NavigatorPay();
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
