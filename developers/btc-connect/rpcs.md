---
description: High-Level RPCs to simplify the integration of BTC Connect
---

# RPCs

\-> [RPC Authentication & chainId config](../node-service/authentication.md)

\-> [Common Account Abstraction RPCs](../account-abstraction/rpc/account-abstraction-rpc.md)

\-> [Bundler RPCs](../account-abstraction/rpc/bundler-rpc.md)

\-> [Paymaster](../account-abstraction/rpc/paymaster-rpc.md)[ RPCs](../account-abstraction/rpc/paymaster-rpc.md)

## particle\_aa\_getBTCAccount

Request

```json
{
  "jsonrpc": "2.0",
  "id": "ee9cce2a-2f34-4c66-879e-c84c6f0e7f2d",
  "chainId": 686868,
  "method": "particle_aa_getBTCAccount",
  "params": [
    // account config array
    {
      "name": "BTC",
      "version": "1.0.0",
      "btcPublicKey": "02b81108a6c83e417056d028c3bfb979dbbcde3164a1d8c77f1efaf5a8d43be31c"
    },
    {
      "name": "BTC",
      "version": "1.0.0",
      "btcPublicKey": "020ec8200effde9ba4d2de890074cda725ba7c1fcc659f7915c0caa96aae7bbadc"
    },
    ,
    {
      "name": "BTC",
      "version": "1.0.0",
      "btcPublicKey": "022a789b682c600068bc06927dd57c52be1daa506ca3036e86f29c9530729b5adb"
    }
  ]
}
```

Response

```json
{
  "jsonrpc": "2.0",
  "id": "c76022e3-0088-4283-ad29-6aac766f1f8f",
  "result": [
    {
      "chainId": 686868,
      "isDeployed": true,
      "eoaAddress": "0x5b3Cc11bBCB5191dC10C39A6D261D42341d982f2",
      "factoryAddress": "0xcf52D4cDb490e47b835E9EA04BAABcA35f40F60e",
      "entryPointAddress": "0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789",
      // this is the btc aa address
      "smartAccountAddress": "0xB1dC48F7D8A7D36F4F244756971bb3A8f39f96bc",
      "owner": "0x5b3Cc11bBCB5191dC10C39A6D261D42341d982f2",
      "name": "BTC",
      "version": "1.0.0",
      "index": 0,
      "btcPublicKey": "02b81108a6c83e417056d028c3bfb979dbbcde3164a1d8c77f1efaf5a8d43be31c"
    },
    {
      "chainId": 686868,
      "isDeployed": false,
      "eoaAddress": "0x83FEddf18cdEa2ea4c087E5F389351171318d3Cb",
      "factoryAddress": "0xcf52D4cDb490e47b835E9EA04BAABcA35f40F60e",
      "entryPointAddress": "0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789",
      // this is the btc aa address
      "smartAccountAddress": "0xFd280E93Cb53b738053d631C0E9d9e057FA8eaD6",
      "owner": "0x83FEddf18cdEa2ea4c087E5F389351171318d3Cb",
      "name": "BTC",
      "version": "1.0.0",
      "index": 0,
      "btcPublicKey": "020ec8200effde9ba4d2de890074cda725ba7c1fcc659f7915c0caa96aae7bbadc"
    },
    {
      "chainId": 686868,
      "isDeployed": true,
      "eoaAddress": "0x4Ade2dBE1eC9eFc8c172Ae0D7528d41EA534B50A",
      "factoryAddress": "0xcf52D4cDb490e47b835E9EA04BAABcA35f40F60e",
      "entryPointAddress": "0x5FF137D4b0FDCD49DcA30c7CF57E578a026d2789",
      // this is the btc aa address
      "smartAccountAddress": "0xE08C3d15ff0201CE5Fa698b647f2BEc41620ee09",
      "owner": "0x4Ade2dBE1eC9eFc8c172Ae0D7528d41EA534B50A",
      "name": "BTC",
      "version": "1.0.0",
      "index": 0,
      "btcPublicKey": "022a789b682c600068bc06927dd57c52be1daa506ca3036e86f29c9530729b5adb"
    }
  ],
  "chainId": 686868
}

```

