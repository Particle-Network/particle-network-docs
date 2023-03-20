---
description: POST /chains/{chainId}/orders
---

# 🔥 Upload order

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

const projectUuId = 'Your Project Id';
const projectKey = 'Your Project Client Or Server Key';
const chainId = 5; // Goerli Network
const baseUrl = 'https://api-market-debug.particle.network';
const url = `${baseUrl}/chains/${chainId}/orders`;

const order = {}; // create from seaport

(async () => {
    const response = await axios.post(url, {
        order,
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
    "success": true
}
```
