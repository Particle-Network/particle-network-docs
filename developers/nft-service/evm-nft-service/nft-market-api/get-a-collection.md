---
description: GET /chains/{chainId}/contractAddress/{contractAddress}
---

# 🔥 Get a Collection

**Results:**

* `<object>` - a collection object

Request example:

{% tabs %}
{% tab title="Node.js" %}
```javascript
const Axios = require("axios");

const projectUuId = 'Your Project Id';
const projectKey = 'Your Project Client Or Server Key';
const chainId = 5; // Goerli Network
const baseUrl = 'https://api-market.particle.network';
const contractAddress = '0xE860aE9379B1902DC08F67F50de7b9CC066AF0FF';
const url = `${baseUrl}/chains/${chainId}/contractAddress/${contractAddress}`;

(async () => {
    const response = await Axios.get(url, {
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
            "contractAddress": "0x8E6eec3901a6647Cab90D061C5DDA74C73D326aB",
            "meta": {
                "name": "Azuki",
                "description": "Azuki starts with a collection of 10,000 avatars that give you membership access to The Garden: a corner of the internet where artists, builders, and web3 enthusiasts meet to create a decentralized future. Azuki holders receive access to exclusive drops, experiences, and more. Visit azuki.com for more details.",
                "image": "https://cdn.x2y2.io/nfts/karbd5r39dtsupt.jpg/280.png",
                "item_width": 716,
                "item_height": 1146,
                "website_url": "https://powerofwomen.xyz/",
                "twitter_url": "https://powerofwomen.xyz/",
                "discord_url": "https://powerofwomen.xyz/",
                "creator_fee_basis_points": 500,
                "creator_fee_recipient": "0x7DDd4989abb3cDa01BA31ac283c194852E42e1a8",
                "banner_url": "https://i.seadn.io/gae/O0XkiR_Z2--OPa_RA6FhXrR16yBOgIJqSLdHTGA0-LAhyzjSYcb3WEPaCYZHeh19JIUEAUazofVKXcY2qOylWCdoeBN6IfGZLJ3I4A?auto=format&w=3840",
                "custom_currency": ["0xB4FBF271143F4FBf7B91A5ded31805e42b2208d6", "0x0000000000000000000000000000000000000000"],
                "custom_sort": ["rarity"],
                "processed_custom_currency": [
                    { "address": "0xB4FBF271143F4FBf7B91A5ded31805e42b2208d6", "symbol": "WETH", "decimals": 18 },
                    { "address": "0x0000000000000000000000000000000000000000", "symbol": "ETH", "decimals": 18 }
                ],
                "processed_custom_sort": ["rarity"]
            },
            "contractType": 2,
            "totalVolumes": [],
            "floorPrices": [],
            "ownerCount": 17,
            "currentSyncedTransferEventAt": 1679294700016,
            "itemCounts": [{ "status": 0, "count": 41 }],
            "createdAt": 1678861891557,
            "updatedAt": 1679294700359,
            "attributeDistribution": [
                {
                    "trait_type": "Brow",
                    "value_distribution": [
                        { "value": "Jianmei", "count": 2 },
                        { "value": "Thin eyebrows", "count": 2 },
                        { "value": "Small curved eyebrows", "count": 9 },
                        { "value": "Brows", "count": 27 }
                    ],
                    "count": 40
                },
                {
                    "trait_type": "Shoes",
                    "value_distribution": [
                        { "value": "Flesh color high heels", "count": 1 },
                        { "value": "Black and white high heels", "count": 13 },
                        { "value": "Red high heels", "count": 4 },
                        { "value": "Orange high heels", "count": 13 },
                        { "value": "Dark blue high heels", "count": 5 },
                        { "value": "Grey sneakers", "count": 4 }
                    ],
                    "count": 40
                },
                {
                    "trait_type": "Upper",
                    "value_distribution": [
                        { "value": "White yellow jacket and black white underwear", "count": 1 },
                        { "value": "Off-white blazer and black and white underwear", "count": 2 },
                        { "value": "White knitted sweater", "count": 8 },
                        { "value": "White-yellow jacket and grey underwear", "count": 3 },
                        { "value": "Dark blue v-neck long sleeves", "count": 9 },
                        { "value": "Orange Long Sleeve Shirt", "count": 12 },
                        { "value": "Black jacket and brown underwear", "count": 2 },
                        { "value": "Off-White Track Jacket", "count": 1 },
                        { "value": "White-yellow jacket and white underwear", "count": 1 }
                    ],
                    "count": 39
                },
                {
                    "trait_type": "Cheek",
                    "value_distribution": [
                        { "value": "Plum red blush", "count": 15 },
                        { "value": "Purple blush", "count": 5 },
                        { "value": "Dark purple blush", "count": 2 },
                        { "value": "Orange blush", "count": 6 },
                        { "value": "Red blush", "count": 9 },
                        { "value": "Pink blush", "count": 1 }
                    ],
                    "count": 38
                },
                {
                    "trait_type": "Bag",
                    "value_distribution": [
                        { "value": "Grey Sports Bag", "count": 2 },
                        { "value": "Flesh-colored leather bag", "count": 1 },
                        { "value": "Black clutch", "count": 10 },
                        { "value": "Fluorescent Sports Bag", "count": 2 },
                        { "value": "Flesh color clutch", "count": 9 },
                        { "value": "Yellow Speckled Clutch", "count": 14 },
                        { "value": "Pink leather bag", "count": 2 }
                    ],
                    "count": 40
                },
                {
                    "trait_type": "Lower",
                    "value_distribution": [
                        { "value": "Black Yoga Pants", "count": 1 },
                        { "value": "Off-White Track Pants", "count": 2 },
                        { "value": "Black dress", "count": 9 },
                        { "value": "Yellow Sweatpants", "count": 1 },
                        { "value": "Zebra print dress", "count": 10 },
                        { "value": "Black shorts", "count": 8 },
                        { "value": "Blue shorts", "count": 7 }
                    ],
                    "count": 38
                },
                {
                    "trait_type": "EyeShadow",
                    "value_distribution": [
                        { "value": "Purple Gradient Eyeshadow", "count": 9 },
                        { "value": "Pink purple eye shadow", "count": 2 },
                        { "value": "Green blue eye shadow", "count": 10 },
                        { "value": "Blue orange red eye shadow", "count": 19 }
                    ],
                    "count": 40
                },
                {
                    "trait_type": "Hair",
                    "value_distribution": [
                        { "value": "Top hat", "count": 8 },
                        { "value": "High ponytail", "count": 1 },
                        { "value": "Low double ponytail", "count": 2 },
                        { "value": "Teenage", "count": 1 },
                        { "value": "Oblique bangs straight - long", "count": 1 },
                        { "value": "Ball head eccentric", "count": 4 },
                        { "value": "Medium wavy", "count": 3 },
                        { "value": "Medium ponytail", "count": 7 },
                        { "value": "Side parted bangs", "count": 13 }
                    ],
                    "count": 40
                },
                {
                    "trait_type": "Lipstick",
                    "value_distribution": [
                        { "value": "Purple lipstick", "count": 2 },
                        { "value": "Big red lipstick", "count": 15 },
                        { "value": "Red lipstick", "count": 21 }
                    ],
                    "count": 38
                },
                {
                    "trait_type": "HairColor",
                    "value_distribution": [
                        { "value": "Dark purple", "count": 1 },
                        { "value": "Dark brown", "count": 3 },
                        { "value": "Purple", "count": 5 },
                        { "value": "Brown", "count": 12 },
                        { "value": "Yellow", "count": 4 },
                        { "value": "Orange blonde", "count": 10 },
                        { "value": "Black", "count": 5 }
                    ],
                    "count": 40
                },
                { "trait_type": "Cloth", "value_distribution": [{ "value": "Black and Yellow Track Jacket", "count": 1 }], "count": 1 }
            ]
        }
    }
}
```
