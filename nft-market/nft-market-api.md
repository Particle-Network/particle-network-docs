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

### 🔥 Gets activitys of a collection

**Query Parameters:**

* `activityTypes: <[string]>` - What types of activities do you want to filter. If not set, all types of data are returned by default
  * activityType
    * sale&#x20;
    * list
    * unlist
    * mint
    * transfer
* `page: <number>` - If not set,`1` by default
* `limit: <number>` - Limit the number of records on a page. If not set,`100` by default

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
```

### 🔥 Gets all the tokens in a collection

**Query Parameters:**

* `page: <number>` - If not set,`1` by default
* `limit: <number>` - Limit the number of records on a page. If not set,`100` by default
*   `query: <JsonString>` - Query the condition as a JSON object and serialize it to a string

    ```json
    {
        // The tokens that meet the conditions are filtered according to the conditions. 
        // The conditions are queried by union
        "filter": {
            // Query NFT with yellow hair and female (Data from metadata)
            "attributes": [
                {
                    "trait_type": "Hair",
                    "value": "Yellow"
                },
                {
                    "trait_type": "Gender",
                    "value": "Women"
                }
            ],
            // Returns tokens owned by the owner
            "ownerAddress": "0x7DDd4989abb3cDa01BA31ac283c194852E42e1a8",
            // Token status: 0 Unlist 1 Listed
            "status": 0
        },
        // Sorting rule
        // The default collation rules supported are price and listedTime, expiredTime
        // Custom collation rules are also supported which defined in ContractURI JSON (custom_sort)
        // asc indicates sequential sorting and desc indicates reverse sorting
        // for example: price_asc, rarity_desc
        "sortBy": "rarity_desc" 
    }
    ```

**Results:**

* `<[object]>` - a token object array

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
    const response = await axios.get(`https://api-market-debug.particle.network/chains/${chainId}/contractAddress/${contractAddress}/tokens`, {
        params: {
            projectUuid,
            projectKey,
            page: 1,
            limit: 10,
            query: JSON.stringify({
                filter: {
                    attributes: [
                        {
                            trait_type: "HairColor",
                            value: "Orange blonde",
                        },
                    ],
                },
                sortBy: "rarity_desc",
            }),
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
        "isLastPage": false,
        "data": [
            {
                "_id": "6409bb608368fa0a2100a241",
                "chainId": 5,
                "contractAddress": "0x8E6eec3901a6647Cab90D061C5DDA74C73D326aB",
                "contractType": 2,
                "tokenId": "27",
                "tokenUri": "https://app-static.particle.network/collections/0x5e763ee03e5a762dd5fa3bd6c507f82311f257cd/27",
                "metadata": {
                    "name": "Women #00035",
                    "description": "To create art that INSPIRES, EMPOWERS and ENABLES women. DIY your first NFT. Create your own image.",
                    "image": "https://ipfs.particle.network/bafybeib7e77le2cfu2kl2nwd25gkuwsucnwwisy3iq4geuym5vz2eutajm",
                    "rarity": 2,
                    "external_link": "https://powerofwomen.xyz",
                    "attributes": [
                        { "trait_type": "Hair", "value": "Oblique bangs straight - long" },
                        { "trait_type": "Cloth", "value": "Black and Yellow Track Jacket" },
                        { "trait_type": "Cheek", "value": "Purple blush" },
                        { "trait_type": "EyeShadow", "value": "Blue orange red eye shadow" },
                        { "trait_type": "Lipstick", "value": "Big red lipstick" },
                        { "trait_type": "HairColor", "value": "Orange blonde" },
                        { "trait_type": "Lower", "value": "Blue shorts" },
                        { "trait_type": "Bag", "value": "Black clutch" },
                        { "trait_type": "Brow", "value": "Small curved eyebrows" },
                        { "trait_type": "Shoes", "value": "Grey sneakers" }
                    ],
                    "properties": { "creators": [{ "address": "0x1e004E19b731239a78eCDcfB6BFb8a54c5ED5094", "share": 100 }] }
                },
                "status": 0,
                "tags": [
                    "attribute:Hair:Oblique bangs straight - long$",
                    "attribute:Cloth:Black and Yellow Track Jacket$",
                    "attribute:Cheek:Purple blush$",
                    "attribute:EyeShadow:Blue orange red eye shadow$",
                    "attribute:Lipstick:Big red lipstick$",
                    "attribute:HairColor:Orange blonde$",
                    "attribute:Lower:Blue shorts$",
                    "attribute:Bag:Black clutch$",
                    "attribute:Brow:Small curved eyebrows$",
                    "attribute:Shoes:Grey sneakers$"
                ],
                "currentOwner": "0xb4025B0319E56F8226D9849589e212D842Eeb942",
                "currentOrderHash": "",
                "currentOrderPrice": "0",
                "currentOrderToken": "",
                "currentOrderTokenSymbol": "",
                "currentOrderTokenDecimals": 0,
                "listedTime": 0,
                "expiredTime": 0,
                "particleCustomScore1": "2",
                "particleCustomScore2": "0",
                "particleCustomScore3": "0",
                "createdAt": 1678359392151,
                "updatedAt": 1678359392151
            }
        ]
    }
}
```

### 🔥 Gets the count of tokens in a collection

**Query Parameters:**

*   `query: <JsonString>` - Query the condition as a JSON object and serialize it to a string

    ```json
    {
        // The tokens that meet the conditions are filtered according to the conditions. 
        // The conditions are queried by union
        "filter": {
            // Query NFT with yellow hair and female (Data from metadata)
            "attributes": [
                {
                    "trait_type": "Hair",
                    "value": "Yellow"
                },
                {
                    "trait_type": "Gender",
                    "value": "Women"
                }
            ],
            // Returns tokens owned by the owner
            "ownerAddress": "0x7DDd4989abb3cDa01BA31ac283c194852E42e1a8",
            // Token status: 0 Unlist 1 Listed
            "status": 0
        },
        // Sorting rule
        // The default collation rules supported are price and listedTime, expiredTime
        // Custom collation rules are also supported which defined in ContractURI JSON (custom_sort)
        // asc indicates sequential sorting and desc indicates reverse sorting
        // for example: price_asc, rarity_desc
        "sortBy": "rarity_desc" 
    }
    ```

**Results:**

* `<object>` - a object contains count

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
    const response = await axios.get(`https://api-market-debug.particle.network/chains/${chainId}/contractAddress/${contractAddress}/tokens/count`, {
        params: {
            projectUuid,
            projectKey,
            page: 1,
            limit: 10,
            query: JSON.stringify({
                filter: {
                    attributes: [
                        {
                            trait_type: "HairColor",
                            value: "Orange blonde",
                        },
                    ],
                },
                sortBy: "rarity_desc",
            }),
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
        "count": 10
    }
}
```

### 🔥 Obtain the details of a token

**Query Parameters:**

* `forceUpdate <boolean string>`: Whether to refresh the metadata of the token
  * true or false string

**Results:**

* `<object>` - a token object

Request example:

{% tabs %}
{% tab title="Node.js" %}
```javascript
const Axios = require("axios");

const chainId = 5; // Goerli Network
const projectUuId = 'Your Project Id';
const projectKey = 'Your Project Client Or Server Key';
const contractAddress = '0xE860aE9379B1902DC08F67F50de7b9CC066AF0FF';
const tokenId = '5';

(async () => {
    const response = await axios.get(`https://api-market-debug.particle.network/chains/${chainId}/contractAddress/${contractAddress}/tokens/${tokenId}`, {
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
        "_id": {},
        "chainId": 5,
        "contractAddress": "0x8E6eec3901a6647Cab90D061C5DDA74C73D326aB",
        "contractType": 2,
        "tokenId": "5",
        "tokenUri": "https://app-static.particle.network/collections/0x5e763ee03e5a762dd5fa3bd6c507f82311f257cd/5",
        "metadata": {
            "name": "Women #00006",
            "description": "To create art that INSPIRES, EMPOWERS and ENABLES women. DIY your first NFT. Create your own image.",
            "image": "https://ipfs.particle.network/bafybeibtxj3brryrbjae7hlskszku6im5bu74muncqtxww26c7gsdvzk5y",
            "rarity": 1,
            "external_link": "https://women.xyz",
            "attributes": [
                { "trait_type": "Hair", "value": "Medium ponytail" },
                { "trait_type": "Upper", "value": "Orange Long Sleeve Shirt" },
                { "trait_type": "HairColor", "value": "Brown" },
                { "trait_type": "Cheek", "value": "Purple blush" },
                { "trait_type": "EyeShadow", "value": "Blue orange red eye shadow" },
                { "trait_type": "Lipstick", "value": "Red lipstick" },
                { "trait_type": "Brow", "value": "Brows" },
                { "trait_type": "Lower", "value": "Zebra print dress" },
                { "trait_type": "Bag", "value": "Yellow Speckled Clutch" },
                { "trait_type": "Shoes", "value": "Black and white high heels" }
            ],
            "properties": { "creators": [{ "address": "0x6fc702d32e6cb268f7dc68766e6b0fe94520499d", "share": 100 }] }
        },
        "status": 1,
        "tags": [
            "attribute:Hair:Medium ponytail$",
            "attribute:Upper:Orange Long Sleeve Shirt$",
            "attribute:HairColor:Brown$",
            "attribute:Cheek:Purple blush$",
            "attribute:EyeShadow:Blue orange red eye shadow$",
            "attribute:Lipstick:Red lipstick$",
            "attribute:Brow:Brows$",
            "attribute:Lower:Zebra print dress$",
            "attribute:Bag:Yellow Speckled Clutch$",
            "attribute:Shoes:Black and white high heels$",
            "status:1$",
            "currentOwner:0x027B35c8D685dfE7e3b2B08970bc8650A14CC58a$"
        ],
        "currentOwner": "0x027B35c8D685dfE7e3b2B08970bc8650A14CC58a",
        "currentOrderHash": "0xc5a55bc63df6d737a84c3872727a16bbe0b1ed117a2466db4ba9f7763f6d08ba",
        "currentOrderPrice": "250000000000000000",
        "currentOrderToken": "0x0000000000000000000000000000000000000000",
        "currentOrderTokenSymbol": "ETH",
        "currentOrderTokenDecimals": 18,
        "listedTime": 1678362294000,
        "expiredTime": 9007199254740991,
        "particleCustomScore1": "1",
        "particleCustomScore2": "0",
        "particleCustomScore3": "0",
        "createdAt": 1678360290788,
        "updatedAt": 1678362316359,
        "currentOrder": {
            "chainId": 5,
            "hash": "0xc5a55bc63df6d737a84c3872727a16bbe0b1ed117a2466db4ba9f7763f6d08ba",
            "protocolAddress": "0x00000000006c3852cbEf3e08E8dF289169EdE581",
            "protocolData": {
                "parameters": {
                    "offerer": "0x027B35c8D685dfE7e3b2B08970bc8650A14CC58a",
                    "zone": "0x0000000000000000000000000000000000000000",
                    "zoneHash": "0x3000000000000000000000000000000000000000000000000000000000000000",
                    "startTime": "1678362294",
                    "endTime": "115792089237316195423570985008687907853269984665640564039457584007913129639935",
                    "orderType": 0,
                    "offer": [
                        {
                            "itemType": 2,
                            "token": "0x8E6eec3901a6647Cab90D061C5DDA74C73D326aB",
                            "identifierOrCriteria": "5",
                            "startAmount": "1",
                            "endAmount": "1"
                        }
                    ],
                    "consideration": [
                        {
                            "itemType": 0,
                            "token": "0x0000000000000000000000000000000000000000",
                            "identifierOrCriteria": "0",
                            "startAmount": "237500000000000000",
                            "endAmount": "237500000000000000",
                            "recipient": "0x027B35c8D685dfE7e3b2B08970bc8650A14CC58a"
                        },
                        {
                            "itemType": 0,
                            "token": "0x0000000000000000000000000000000000000000",
                            "identifierOrCriteria": "0",
                            "startAmount": "12500000000000000",
                            "endAmount": "12500000000000000",
                            "recipient": "0x7DDd4989abb3cDa01BA31ac283c194852E42e1a8"
                        }
                    ],
                    "totalOriginalConsiderationItems": 2,
                    "salt": "0x0000000017673d72d29cb506",
                    "conduitKey": "0x0000000000000000000000000000000000000000000000000000000000000000",
                    "counter": 0
                },
                "signature": "0x7d1c496b54f98f42ab4aaced7a5cd4172cd33d3898ba6538e4c75bec26295e0f63406788ba01c0736e7ce9df4062ab419b1448234c05a4939a9a6bac34913fe7"
            },
            "type": "listing",
            "status": 0,
            "associatedTokens": [{ "tokenAddress": "0x8E6eec3901a6647Cab90D061C5DDA74C73D326aB", "tokenId": "5", "_id": {} }],
            "_id": {},
            "createdAt": "2023-03-09T11:45:16.327Z",
            "updatedAt": "2023-03-09T11:45:16.328Z"
        }
    }
}
```

