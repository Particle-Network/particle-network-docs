---
description: GET /chains/{chainId}/contractAddress/{contractAddress}/tokens/{tokenId}
---

# 🔥 Get single Token info

**Query Parameters:**

* `forceUpdate <boolean string>`: Whether to refresh the metadata of the token
  * true or false `<string>`

**Results:**

* `<object>` - a token object

Request example:

{% tabs %}
{% tab title="Node.js" %}
```javascript
const Axios = require("axios");

const projectUuId = 'Your Project Id';
const projectKey = 'Your Project Client Or Server Key';
const chainId = 5; // Goerli Network
const tokenId = '5';
const baseUrl = 'https://api-market-debug.particle.network';
const contractAddress = '0x14CC6aF7A7318347419758274049e93DB03236C9';
const url = `${baseUrl}/chains/${chainId}/contractAddress/${contractAddress}/tokens/${tokenId}`;

(async () => {
    const response = await axios.get(url, {
        params: {
            projectUuid,
            projectKey,
            forceUpdate: 'true',
        },
    });

    console.log(JSON.stringify(response.data));
})();
```
{% endtab %}
{% endtabs %}

Response example:

```json
{
    "error_code": 0,
    "data": {
        "isLastPage": true,
        "data": {
            "_id": "6411663623f12d36b8a67a13",
            "chainId": 5,
            "contractAddress": "0x14CC6aF7A7318347419758274049e93DB03236C9",
            "contractType": 2,
            "tokenId": "0",
            "tokenUri": "https://ikzttp.mypinata.cloud/ipfs/QmQFkLSQysj94s5GvTHPyzTxrawwtjgiiYS2TBLgrvw8CW/0",
            "metadata": {
                "name": "Azuki #0",
                "image": "https://ikzttp.mypinata.cloud/ipfs/QmYDvPAXtiJg7s8JdRBSLWdgSphQdac8j1YuQNNxcGE1hg/0.png",
                "attributes": [
                    { "trait_type": "Type", "value": "Human" },
                    { "trait_type": "Hair", "value": "Water" },
                    { "trait_type": "Clothing", "value": "Pink Oversized Kimono" },
                    { "trait_type": "Eyes", "value": "Striking" },
                    { "trait_type": "Mouth", "value": "Frown" },
                    { "trait_type": "Offhand", "value": "Monkey King Staff" },
                    { "trait_type": "Background", "value": "Off White A" }
                ]
            },
            "status": 1,
            "tags": [
                "attribute|Type|Human$",
                "attribute|Hair|Water$",
                "attribute|Clothing|Pink Oversized Kimono$",
                "attribute|Eyes|Striking$",
                "attribute|Mouth|Frown$",
                "attribute|Offhand|Monkey King Staff$",
                "attribute|Background|Off White A$",
                "status|1$",
                "currentOwner|0x7b9A20a2A3260af07668f467FBAE20A536B204Eb$"
            ],
            "currentOwner": "0x7b9A20a2A3260af07668f467FBAE20A536B204Eb",
            "currentOrderHash": "0x549999a18888bd60d5a4b9b6496676034a8db019b60cab4d4a1a87dc637ada49",
            "currentOrderPrice": "1000000000000000000",
            "currentOrderToken": "0x0000000000000000000000000000000000000000",
            "currentOrderTokenSymbol": "ETH",
            "currentOrderTokenDecimals": 18,
            "listedTime": 1679272945000,
            "expiredTime": 9007199254740991,
            "particleCustomScore1": "0",
            "particleCustomScore2": "0",
            "particleCustomScore3": "0",
            "createdAt": 1678861878366,
            "updatedAt": 1679272980548,
            "currentOrder": {
                "chainId": 5,
                "hash": "0x549999a18888bd60d5a4b9b6496676034a8db019b60cab4d4a1a87dc637ada49",
                "protocolAddress": "0x00000000000001ad428e4906aE43D8F9852d0dD6",
                "protocolData": {
                    "parameters": {
                        "offerer": "0x7b9A20a2A3260af07668f467FBAE20A536B204Eb",
                        "zone": "0x0000000000000000000000000000000000000000",
                        "zoneHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
                        "startTime": "1679272945",
                        "endTime": "115792089237316195423570985008687907853269984665640564039457584007913129639935",
                        "orderType": 0,
                        "offer": [
                            {
                                "itemType": 2,
                                "token": "0x14CC6aF7A7318347419758274049e93DB03236C9",
                                "identifierOrCriteria": "0",
                                "startAmount": "1",
                                "endAmount": "1"
                            }
                        ],
                        "consideration": [
                            {
                                "itemType": 0,
                                "token": "0x0000000000000000000000000000000000000000",
                                "identifierOrCriteria": "0",
                                "startAmount": "940000000000000000",
                                "endAmount": "940000000000000000",
                                "recipient": "0x7b9A20a2A3260af07668f467FBAE20A536B204Eb"
                            },
                            {
                                "itemType": 0,
                                "token": "0x0000000000000000000000000000000000000000",
                                "identifierOrCriteria": "0",
                                "startAmount": "60000000000000000",
                                "endAmount": "60000000000000000",
                                "recipient": "0x7DDd4989abb3cDa01BA31ac283c194852E42e1a8"
                            }
                        ],
                        "totalOriginalConsiderationItems": 2,
                        "salt": "0x00000000a4e7ecd5d0212a12",
                        "conduitKey": "0x0000000000000000000000000000000000000000000000000000000000000000",
                        "counter": "0"
                    },
                    "signature": "0x4b8ba9097a92979fba84ffdb32f32a1076c4c6ef625379b006dd652506208b896d330ae75add9871b0fca3dc9baf6801c3b151f542698cbe9f61a1c4a62aa1fd"
                },
                "type": "listing",
                "status": 0,
                "associatedTokens": [{ "tokenAddress": "0x14CC6aF7A7318347419758274049e93DB03236C9", "tokenId": "0", "_id": {} }],
                "_id": {},
                "createdAt": "2023-03-20T00:43:00.520Z",
                "updatedAt": "2023-03-20T00:43:00.523Z"
            },
            "lastSale": {
                "orderPrice": "20000000000000000",
                "orderToken": "0x0000000000000000000000000000000000000000",
                "orderTokenDecimals": 18,
                "orderTokenSymbol": "ETH"
            }
        }
    }
}
```
