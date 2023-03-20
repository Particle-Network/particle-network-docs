---
description: POST /chains/{chainId}/contractAddress/{contractAddress}/refresh
---

# 🔥 Refresh the Collection metadata

**Results:**

* `<object>`&#x20;
  * `success: <boolean>`: Is the refresh successful

Request example:

{% tabs %}
{% tab title="Node.js" %}
```javascript
const Axios = require("axios");

const projectUuId = 'Your Project Id';
const projectKey = 'Your Project Client Or Server Key';
const chainId = 5; // Goerli Network
const baseUrl = 'https://api-market-debug.particle.network';
const contractAddress = '0xE860aE9379B1902DC08F67F50de7b9CC066AF0FF';
const url = `${baseUrl}/chains/${chainId}/contractAddress/${contractAddress}/refresh`;

(async () => {
    const response = await Axios.post(url, {
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
        "success": true
    }
}
```
