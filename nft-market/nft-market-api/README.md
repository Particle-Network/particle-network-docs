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

### 🔥 Upload order

{% hint style="info" %}
The Particle Market currently only supports [the Seaport protocol](https://github.com/ProjectOpenSea/seaport)

Uploaded orders only support single NFT purchases
{% endhint %}

**Body Parameters:**

* `order <seaport order struct>`: An order created through seaport Which contains `parameters` and `signature`

**Results:**

* `<object>`

Request example:

{% tabs %}
{% tab title="Node.js" %}
```javascript
const Axios = require("axios");

const chainId = 5; // Goerli Network
const projectUuId = 'Your Project Id';
const projectKey = 'Your Project Client Or Server Key';
const tokenId = '5';
const order = {}; // create from seaport

(async () => {
    const response = await axios.get(`https://api-market-debug.particle.network/chains/${chainId}/orders`, {
        params: {
            projectUuid,
            projectKey,
            order,
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
    "success": true
}
```
