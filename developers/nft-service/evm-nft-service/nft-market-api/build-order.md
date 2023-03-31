---
description: POST /chains/{chainId}/orders/build
---

# 🔥 Build order

{% hint style="info" %}
The Particle Market currently only supports [the Seaport protocol](https://github.com/ProjectOpenSea/seaport)

\
For users who are using JavaScript, we **highly recommend** using [seaport-js](https://github.com/ProjectOpenSea/seaport-js) to generate orders.\


Considering that some users may use languages other than JavaScript and find it inconvenient to create orders in compliance with the seaport standard, we have added relevant APIs on the server-side.
{% endhint %}

**Body Parameters:**

* `fromAddress <address>`: NFT owner
* `tokenAddress <address>`: NFT contract address
* `tokenId <string>`: NFT TokenId
* `price <string>`: The price of an NFT is denominated in the smallest unit of the currency, such as Wei for ETH.
* `currency <address, optional>`: Currency address - If the currency is an ERC20 token, enter the token's address here. If it is a native token, enter `0x0000000000000000000000000000000000000000`. If not entered, the default is `0x0000000000000000000000000000000000000000`.
  * Note: **The currency address must be within the currency scope defined in the contract metadata.**

**Results:**

* `<object>`: transactions or message to sign

Request example:

{% tabs %}
{% tab title="Node.js" %}
```javascript
const Axios = require("axios");

const projectUuId = 'Your Project Id';
const projectKey = 'Your Project Client Or Server Key';
const chainId = 5; // Goerli Network
const baseUrl = 'https://api-market.particle.network';
const url = `${baseUrl}/chains/${chainId}/orders/build`;

const fromAddress = '0x3E08c246D9AD2091c549E73D622A8C3c9d114fD7';
const tokenAddress = '0x8e6eec3901a6647cab90d061c5dda74c73d326ab';
const tokenId = '64';
const price = '1000000000'; // wei

(async () => {
    const response = await axios.post(url, {
        fromAddress,
        tokenAddress,
        tokenId,
        price,
    }, {
        params: {
            projectUuid,
            projectKey,
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
    "data": [
        // For first-time sellers, it is necessary to 
        // first authorize the transfer of the NFT to the Seaport protocol.
        {
            "type": "approval",
            "tx": {
                "data": "0xa22cb46500000000000000000000000000000000000001ad428e4906ae43d8f9852d0dd60000000000000000000000000000000000000000000000000000000000000001c5d24601",
                "to": "0x8E6eec3901a6647Cab90D061C5DDA74C73D326aB",
                "from": "0x3E08c246D9AD2091c549E73D622A8C3c9d114fD7"
            }
        },
        // The order structure is a signable EIP-712 structure, 
        // signed using the signTypedDataV4 method to generate a signature.
        // order parameters = JSON.parse(typedDataV4MessageToSign).message
        // order signature = signTypedDataV4 with typedDataV4ToSign
        {
            "type": "create",
            "typedDataV4ToSign": {
                "types": {
                    "OrderComponents": [
                        { "name": "offerer", "type": "address" },
                        { "name": "zone", "type": "address" },
                        { "name": "offer", "type": "OfferItem[]" },
                        { "name": "consideration", "type": "ConsiderationItem[]" },
                        { "name": "orderType", "type": "uint8" },
                        { "name": "startTime", "type": "uint256" },
                        { "name": "endTime", "type": "uint256" },
                        { "name": "zoneHash", "type": "bytes32" },
                        { "name": "salt", "type": "uint256" },
                        { "name": "conduitKey", "type": "bytes32" },
                        { "name": "counter", "type": "uint256" }
                    ],
                    "OfferItem": [
                        { "name": "itemType", "type": "uint8" },
                        { "name": "token", "type": "address" },
                        { "name": "identifierOrCriteria", "type": "uint256" },
                        { "name": "startAmount", "type": "uint256" },
                        { "name": "endAmount", "type": "uint256" }
                    ],
                    "ConsiderationItem": [
                        { "name": "itemType", "type": "uint8" },
                        { "name": "token", "type": "address" },
                        { "name": "identifierOrCriteria", "type": "uint256" },
                        { "name": "startAmount", "type": "uint256" },
                        { "name": "endAmount", "type": "uint256" },
                        { "name": "recipient", "type": "address" }
                    ],
                    "EIP712Domain": [
                        { "name": "name", "type": "string" },
                        { "name": "version", "type": "string" },
                        { "name": "chainId", "type": "uint256" },
                        { "name": "verifyingContract", "type": "address" }
                    ]
                },
                "domain": {
                    "name": "Seaport",
                    "version": "1.4",
                    "chainId": "5",
                    "verifyingContract": "0x00000000000001ad428e4906ae43d8f9852d0dd6"
                },
                "primaryType": "OrderComponents",
                "message": {
                    "offerer": "0x3e08c246d9ad2091c549e73d622a8c3c9d114fd7",
                    "zone": "0x0000000000000000000000000000000000000000",
                    "offer": [
                        {
                            "itemType": "2",
                            "token": "0x8e6eec3901a6647cab90d061c5dda74c73d326ab",
                            "identifierOrCriteria": "64",
                            "startAmount": "1",
                            "endAmount": "1"
                        }
                    ],
                    "consideration": [
                        {
                            "itemType": "0",
                            "token": "0x0000000000000000000000000000000000000000",
                            "identifierOrCriteria": "0",
                            "startAmount": "950000000",
                            "endAmount": "950000000",
                            "recipient": "0x3e08c246d9ad2091c549e73d622a8c3c9d114fd7"
                        },
                        {
                            "itemType": "0",
                            "token": "0x0000000000000000000000000000000000000000",
                            "identifierOrCriteria": "0",
                            "startAmount": "50000000",
                            "endAmount": "50000000",
                            "recipient": "0x7ddd4989abb3cda01ba31ac283c194852e42e1a8"
                        }
                    ],
                    "orderType": "0",
                    "startTime": "1680252858",
                    "endTime": "115792089237316195423570985008687907853269984665640564039457584007913129639935",
                    "zoneHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
                    "salt": "455460793455882038",
                    "conduitKey": "0x0000000000000000000000000000000000000000000000000000000000000000",
                    "counter": "0"
                }
            }
        }
    ]
}
```
