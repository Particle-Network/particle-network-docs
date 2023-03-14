---
description: Get the real-time data
---

# NFT Market API

### Base URL

**https://api-market.particle.network**

### Authentication

Accessing the Market API requires passing the user's project information.

For example:

{% tabs %}
{% tab title="Node.js" %}
```javascript
const Axios = require('axios');

const chainId = 5; // Goerli Network
const projectId = 'Your Project Id';
const projectServerKey = 'Your Project Client Or Server Key';
const baseUrl = 'https://api-market-debug.particle.network';

(async () => {
    const response = await Axios.get(`${baseUrl}/chains/${chainId}/contractAddress?projectUuid=${projectId}&projectKey=${projectServerKey}`);

    console.log(response.data);
})();
```
{% endtab %}
{% endtabs %}
