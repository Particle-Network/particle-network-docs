---
description: Integrate Swap seamlessly into your dApp
---

# Swap

## How to use

1. If from token is ERC20 token, need to check if user has approved enough amount for the swap contract to use by calling `particle_swap_checkApprove`
2. (Optional) Sign, send and wait for the approve tx confirmed
3. Get quote by calling `particle_swap_getQuote`
4. Get swap by calling `particle_swap_getSwap`
5. Sign, send and wait for the swap tx confirmed

📢 For Native token like ETH, use this address\
`0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee`

## Supported Chains

1. Ethereum Mainnet
2. BNB Chain Mainnet
3. Polygon Mainnet
4. Optimism Mainnet
5. Arbitrum One
6. Avalanche C-Chain Mainnet
7. Fantom Mainnet
8. Aurora Mainnet
9. Gnosis Mainnet
10. Klaytn Mainet
11. zkSync Era Mainnet
12. Mantle Mainnet
13. OKTC Mainnet
14. Moonriver Mainnet
15. Moonbeam Mainnet
16. Celo Mainnet
17. Cronos Mainnet

## Underlying DEXs

1. 1inch: [https://1inch.io/](https://1inch.io/)
2. izi-swap: [https://izumi.finance/](https://izumi.finance/)
3. lifi: [https://li.fi/](https://li.fi/)

## particle\_swap\_checkApprove

Request

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "particle_swap_checkApprove",
  "params": [
    "0x369aa8a7a7BE683E1a46d9A056806B2B3FD778C8", // wallet address
    {
      "tokenAddress": "0x111111111117dc0aa78b770fa6a738034120c302", // from token address
      "amount": "1000000000"
    }
  ]
}
```

Response

```json
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "approved": true
  },
  "chainId": 1
}
```

```json
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "approved": false,
    "tx": { // Construct a complete transaction on the client side.
      "to": "0x111111111117dC0aa78b770fA6A738034120C302",
      "data": "0x095ea7b30000000000000000000000001111111254fb6c44bac0bed2854e76f90643097d000000000000000000000000000000000000000000000000000000003b9aca00",
      "value": "0x0",
    }
  },
  "chainId": 1
}
```

## particle\_swap\_getQuote

Request

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "particle_swap_getQuote",
  "params": [
    "0x369aa8a7a7BE683E1a46d9A056806B2B3FD778C8", // wallet address
    {
      "fromTokenAddress": "0x111111111117dc0aa78b770fa6a738034120c302",
      "toTokenAddress": "0x6B175474E89094C44Da98b954EedeAC495271d0F",
      "amount": "1000000000"
    }
  ]
}
```

Response

```json
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "fromToken": {
      "chainId": 1,
      "address": "0x111111111117dC0aa78b770fA6A738034120C302",
      "symbol": "1INCH",
      "name": "1INCHToken",
      "decimals": 18,
      "logoURI": "https://static.particle.network/token-list/ethereum/0x111111111117dC0aa78b770fA6A738034120C302.png"
    },
    "toToken": {
      "chainId": 1,
      "address": "0x6B175474E89094C44Da98b954EedeAC495271d0F",
      "symbol": "DAI",
      "name": "DaiStablecoin",
      "decimals": 18,
      "logoURI": "https://static.particle.network/token-list/ethereum/0x6B175474E89094C44Da98b954EedeAC495271d0F.png"
    },
    "toTokenAmount": "781970036",
    "fromTokenAmount": "1000000000",
    "estimatedGas": "0x4079f"
  },
  "chainId": 1
}
```

## particle\_swap\_getSwap

Request

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "particle_swap_getSwap",
  "params": [
    "0x369aa8a7a7BE683E1a46d9A056806B2B3FD778C8",
    {
      "fromTokenAddress": "0x111111111117dc0aa78b770fa6a738034120c302",
      "toTokenAddress": "0x6B175474E89094C44Da98b954EedeAC495271d0F",
      "amount": "1000000000",
      "slippage": 1
    }
  ]
}
```

Response

```json
{
  "jsonrpc": "2.0",
  "id": 0,
  "result": {
    "fromToken": {
      "chainId": 1,
      "address": "0x111111111117dC0aa78b770fA6A738034120C302",
      "symbol": "1INCH",
      "name": "1INCHToken",
      "decimals": 18,
      "logoURI": "https://static.particle.network/token-list/ethereum/0x111111111117dC0aa78b770fA6A738034120C302.png"
    },
    "toToken": {
      "chainId": 1,
      "address": "0x6B175474E89094C44Da98b954EedeAC495271d0F",
      "symbol": "DAI",
      "name": "DaiStablecoin",
      "decimals": 18,
      "logoURI": "https://static.particle.network/token-list/ethereum/0x6B175474E89094C44Da98b954EedeAC495271d0F.png"
    },
    "toTokenAmount": "781970036",
    "fromTokenAmount": "1000000000",
    "tx": { // Construct a complete transaction on the client side.
      "to": "0x1111111254fb6c44bAC0beD2854e76F90643097d",
      "data": "0x000",
      "value": "0x0",
    }
  },
  "chainId": 1
}
```
