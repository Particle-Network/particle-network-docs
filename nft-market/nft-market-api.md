---
description: Get the real-time data
---

# NFT Market API

### Base URL

**https://api-market.particle.network**

### Authentication

Accessing the Market API requires passing the user's project information in query parameters.

For example:

{% tabs %}
{% tab title="Node.js" %}
```javascript
const Axios = require("axios");

const chainId = 5; // Goerli Network
const projectUuId = 'Your Project Id';
const projectKey = 'Your Project Client Or Server Key';

(async () => {
    const response = await Axios.get(`https://api-market-debug.particle.network/chains/${chainId}/contractAddress`, {
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

### 🔥 Get the collection list

**Query Parameters:**

* `page <number>` - If not set,`1` by default
* `limit <number>` - Limit the number of records on a page. If not set,`100` by default

**Results:**

* `<[object]>` - a collection object array

Request example:

{% tabs %}
{% tab title="Node.js" %}
```javascript
const Axios = require("axios");

const chainId = 5; // Goerli Network
const projectUuId = 'Your Project Id';
const projectKey = 'Your Project Client Or Server Key';

(async () => {
    const response = await Axios.get(`https://api-market-debug.particle.network/chains/${chainId}/contractAddress`, {
        params: {
            projectUuid,
            projectKey,
            page: 1,
            limit: 10,
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
        "data": [
            {
                "_id": {},
                "chainId": 5,
                "contractAddress": "0x14CC6aF7A7318347419758274049e93DB03236C9",
                "meta": {
                    "name": "Meka Drivers",
                    "description": "Join the #MEKAGANG and become a part of our epic Meka community! Get exclusive access to the World Map and earn rewards. Experience the ultimate gaming thrill with our partnership CoreGames, where you have the power to control your own Driver. Customize your Driver in-game in the immersive Meka Lore universe and be ready for even more features to come!",
                    "image": "https://i.seadn.io/gcs/files/bde660047ec7e69ee6b74174d63cf1ff.gif?auto=format&w=2048",
                    "website_url": "https://powerofwomen.xyz/",
                    "twitter_url": "https://powerofwomen.xyz/",
                    "discord_url": "https://powerofwomen.xyz/",
                    "creator_fee_basis_points": 600,
                    "creator_fee_recipient": "0x7DDd4989abb3cDa01BA31ac283c194852E42e1a8",
                    "banner_url": "https://i.seadn.io/gcs/files/0f490fe5ff953443a16823343dacce31.jpg?auto=format&w=2048",
                    "custom_currency": ["0xC51FceEc013cD34aE2e95E6D64E9858F2aC28fFf", "0x0000000000000000000000000000000000000000"],
                    "custom_sort": ["rarity"],
                    "processed_custom_currency": [
                        { "address": "0xC51FceEc013cD34aE2e95E6D64E9858F2aC28fFf", "symbol": "USDT", "decimals": 18 },
                        { "address": "0x0000000000000000000000000000000000000000", "symbol": "ETH", "decimals": 18 }
                    ],
                    "processed_custom_sort": ["rarity"]
                },
                "contractType": 2,
                "totalVolumes": [],
                "floorPrices": [
                    { "token": "0x0000000000000000000000000000000000000000", "symbol": "ETH", "decimals": 18, "price": "1000000000000000" }
                ],
                "ownerCount": 1,
                "currentSyncedTransferEventAt": 1678783320022,
                "itemCounts": [
                    { "status": 1, "count": 1 },
                    { "status": 0, "count": 5 }
                ],
                "createdAt": 1678362928298,
                "updatedAt": 1678783320335,
                "attributeDistribution": [
                    {
                        "trait_type": "Hair",
                        "value_distribution": [
                            { "value": "Pink Flowy", "count": 1 },
                            { "value": "Pink Hairband", "count": 1 },
                            { "value": "Blonde Swept Back", "count": 1 },
                            { "value": "Green Spiky", "count": 1 },
                            { "value": "Brown Dreadlocks", "count": 1 },
                            { "value": "Water", "count": 1 }
                        ],
                        "count": 6
                    }
                ]
            }
        ]
    }
}
```

### 🔥 Get a collection

**Results:**

* `<object>` - a collection object

Request example:

{% tabs %}
{% tab title="Node.js" %}
```javascript
const Axios = require("axios");

const chainId = 5; // Goerli Network
const projectUuId = 'Your Project Id';
const projectKey = 'Your Project Client Or Server Key';
const contractAddress = '0xE860aE9379B1902DC08F67F50de7b9CC066AF0FF';

(async () => {
    const response = await Axios.get(`https://api-market-debug.particle.network/chains/${chainId}/contractAddress/${contractAddress}`, {
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
    "data": {
        "isLastPage": true,
        "data": {
            "_id": {},
            "chainId": 5,
            "contractAddress": "0x14CC6aF7A7318347419758274049e93DB03236C9",
            "meta": {
                "name": "Meka Drivers",
                "description": "Join the #MEKAGANG and become a part of our epic Meka community! Get exclusive access to the World Map and earn rewards. Experience the ultimate gaming thrill with our partnership CoreGames, where you have the power to control your own Driver. Customize your Driver in-game in the immersive Meka Lore universe and be ready for even more features to come!",
                "image": "https://i.seadn.io/gcs/files/bde660047ec7e69ee6b74174d63cf1ff.gif?auto=format&w=2048",
                "website_url": "https://powerofwomen.xyz/",
                "twitter_url": "https://powerofwomen.xyz/",
                "discord_url": "https://powerofwomen.xyz/",
                "creator_fee_basis_points": 600,
                "creator_fee_recipient": "0x7DDd4989abb3cDa01BA31ac283c194852E42e1a8",
                "banner_url": "https://i.seadn.io/gcs/files/0f490fe5ff953443a16823343dacce31.jpg?auto=format&w=2048",
                "custom_currency": ["0xC51FceEc013cD34aE2e95E6D64E9858F2aC28fFf", "0x0000000000000000000000000000000000000000"],
                "custom_sort": ["rarity"],
                "processed_custom_currency": [
                    { "address": "0xC51FceEc013cD34aE2e95E6D64E9858F2aC28fFf", "symbol": "USDT", "decimals": 18 },
                    { "address": "0x0000000000000000000000000000000000000000", "symbol": "ETH", "decimals": 18 }
                ],
                "processed_custom_sort": ["rarity"]
            },
            "contractType": 2,
            "totalVolumes": [],
            "floorPrices": [
                { "token": "0x0000000000000000000000000000000000000000", "symbol": "ETH", "decimals": 18, "price": "1000000000000000" }
            ],
            "ownerCount": 1,
            "currentSyncedTransferEventAt": 1678783320022,
            "itemCounts": [
                { "status": 1, "count": 1 },
                { "status": 0, "count": 5 }
            ],
            "createdAt": 1678362928298,
            "updatedAt": 1678783320335,
            "attributeDistribution": [
                {
                    "trait_type": "Hair",
                    "value_distribution": [
                        { "value": "Pink Flowy", "count": 1 },
                        { "value": "Pink Hairband", "count": 1 },
                        { "value": "Blonde Swept Back", "count": 1 },
                        { "value": "Green Spiky", "count": 1 },
                        { "value": "Brown Dreadlocks", "count": 1 },
                        { "value": "Water", "count": 1 }
                    ],
                    "count": 6
                }
            ]
        }
    }
}
```

### 🔥 Gets the activity record of a collection

**Query Parameters:**

* `actionType:`&#x20;
* `page <number>` - If not set,`1` by default
* `limit <number>` - Limit the number of records on a page. If not set,`100` by default

**Results:**

* `<object>` - a collection object

Request example:

{% tabs %}
{% tab title="Node.js" %}
```javascript
const Axios = require("axios");

const chainId = 5; // Goerli Network
const projectUuId = 'Your Project Id';
const projectKey = 'Your Project Client Or Server Key';
const contractAddress = '0xE860aE9379B1902DC08F67F50de7b9CC066AF0FF';

(async () => {
    const response = await Axios.get(`https://api-market-debug.particle.network/chains/${chainId}/contractAddress/${contractAddress}`, {
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
    "data": {
        "isLastPage": true,
        "data": {
            "_id": {},
            "chainId": 5,
            "contractAddress": "0x14CC6aF7A7318347419758274049e93DB03236C9",
            "meta": {
                "name": "Meka Drivers",
                "description": "Join the #MEKAGANG and become a part of our epic Meka community! Get exclusive access to the World Map and earn rewards. Experience the ultimate gaming thrill with our partnership CoreGames, where you have the power to control your own Driver. Customize your Driver in-game in the immersive Meka Lore universe and be ready for even more features to come!",
                "image": "https://i.seadn.io/gcs/files/bde660047ec7e69ee6b74174d63cf1ff.gif?auto=format&w=2048",
                "website_url": "https://powerofwomen.xyz/",
                "twitter_url": "https://powerofwomen.xyz/",
                "discord_url": "https://powerofwomen.xyz/",
                "creator_fee_basis_points": 600,
                "creator_fee_recipient": "0x7DDd4989abb3cDa01BA31ac283c194852E42e1a8",
                "banner_url": "https://i.seadn.io/gcs/files/0f490fe5ff953443a16823343dacce31.jpg?auto=format&w=2048",
                "custom_currency": ["0xC51FceEc013cD34aE2e95E6D64E9858F2aC28fFf", "0x0000000000000000000000000000000000000000"],
                "custom_sort": ["rarity"],
                "processed_custom_currency": [
                    { "address": "0xC51FceEc013cD34aE2e95E6D64E9858F2aC28fFf", "symbol": "USDT", "decimals": 18 },
                    { "address": "0x0000000000000000000000000000000000000000", "symbol": "ETH", "decimals": 18 }
                ],
                "processed_custom_sort": ["rarity"]
            },
            "contractType": 2,
            "totalVolumes": [],
            "floorPrices": [
                { "token": "0x0000000000000000000000000000000000000000", "symbol": "ETH", "decimals": 18, "price": "1000000000000000" }
            ],
            "ownerCount": 1,
            "currentSyncedTransferEventAt": 1678783320022,
            "itemCounts": [
                { "status": 1, "count": 1 },
                { "status": 0, "count": 5 }
            ],
            "createdAt": 1678362928298,
            "updatedAt": 1678783320335,
            "attributeDistribution": [
                {
                    "trait_type": "Hair",
                    "value_distribution": [
                        { "value": "Pink Flowy", "count": 1 },
                        { "value": "Pink Hairband", "count": 1 },
                        { "value": "Blonde Swept Back", "count": 1 },
                        { "value": "Green Spiky", "count": 1 },
                        { "value": "Brown Dreadlocks", "count": 1 },
                        { "value": "Water", "count": 1 }
                    ],
                    "count": 6
                }
            ]
        }
    }
}
```

