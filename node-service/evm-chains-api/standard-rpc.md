# Standard RPC

The full list of API methods that are supported by is given below. And an error returned if a method is specified that is not supported.

For a full list of RPC API methods, refer to the [JSON-RPC specification](https://github.com/ethereum/execution-apis).

| RPC API method                           | Cloudflare Ethereum Gateway support |
| ---------------------------------------- | ----------------------------------- |
| web3\_clientVersion                      | ✅                                   |
| web3\_sha3                               | ✅                                   |
| net\_version                             | ✅                                   |
| net\_peerCount                           | ❌                                   |
| net\_listening                           | ❌                                   |
| eth\_protocolVersion                     | ✅                                   |
| eth\_syncing                             | ✅                                   |
| eth\_coinbase                            | ❌                                   |
| eth\_mining                              | ✅                                   |
| eth\_hashrate                            | ❌                                   |
| eth\_gasPrice                            | ✅                                   |
| eth\_feeHistory                          | ❌                                   |
| eth\_accounts                            | ❌                                   |
| eth\_blockNumber                         | ✅                                   |
| eth\_chainId                             | ✅                                   |
| eth\_getBalance\*                        | ✅                                   |
| eth\_getStorageAt\*                      | ✅                                   |
| eth\_getTransactionCount\*               | ✅                                   |
| eth\_getBlockTransactionCountByHash      | ✅                                   |
| eth\_getBlockTransactionCountByNumber    | ✅                                   |
| eth\_getUncleCountByBlockHash            | ✅                                   |
| eth\_getUncleCountByBlockNumber          | ✅                                   |
| eth\_getCode\*                           | ✅                                   |
| eth\_sign                                | ❌                                   |
| eth\_sendTransaction                     | ❌                                   |
| eth\_sendRawTransaction                  | ✅                                   |
| eth\_call\*                              | ✅                                   |
| eth\_estimateGas                         | ✅                                   |
| eth\_getBlockByHash                      | ✅                                   |
| eth\_getBlockByNumber                    | ✅                                   |
| eth\_getTransactionByHash                | ✅                                   |
| eth\_getTransactionByBlockHashAndIndex   | ✅                                   |
| eth\_getTransactionByBlockNumberAndIndex | ✅                                   |
| eth\_getTransactionReceipt               | ✅                                   |
| eth\_getUncleByBlockHashAndIndex         | ✅                                   |
| eth\_getUncleByBlockNumberAndIndex       | ✅                                   |
| eth\_getCompilers                        | ❌                                   |
| eth\_compileLLL                          | ❌                                   |
| eth\_compileSolidity                     | ❌                                   |
| eth\_compileSerpent                      | ❌                                   |
| eth\_newFilter                           | ❌                                   |
| eth\_newBlockFilter                      | ❌                                   |
| eth\_newPendingTransactionFilter         | ❌                                   |
| eth\_uninstallFilter                     | ❌                                   |
| eth\_getFilterChanges                    | ❌                                   |
| eth\_getFilterLogs                       | ❌                                   |
| eth\_getLogs\*                           | ✅                                   |
| eth\_getWork                             | ✅                                   |
| eth\_submitWork                          | ✅                                   |
| eth\_submitHashrate                      | ✅                                   |
| eth\_getProof                            | ✅                                   |

{% hint style="info" %}
RPC API methods followed by “\*” are only supported for the latest 128 blocks
{% endhint %}
