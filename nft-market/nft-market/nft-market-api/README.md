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

## API

{% content-ref url="track-a-collection.md" %}
[track-a-collection.md](track-a-collection.md)
{% endcontent-ref %}

{% content-ref url="get-the-collection-list.md" %}
[get-the-collection-list.md](get-the-collection-list.md)
{% endcontent-ref %}

{% content-ref url="get-a-collection.md" %}
[get-a-collection.md](get-a-collection.md)
{% endcontent-ref %}

{% content-ref url="gets-activities-of-a-collection.md" %}
[gets-activities-of-a-collection.md](gets-activities-of-a-collection.md)
{% endcontent-ref %}

{% content-ref url="gets-activities-of-a-collection-by-address.md" %}
[gets-activities-of-a-collection-by-address.md](gets-activities-of-a-collection-by-address.md)
{% endcontent-ref %}

{% content-ref url="refresh-the-collection-metadata.md" %}
[refresh-the-collection-metadata.md](refresh-the-collection-metadata.md)
{% endcontent-ref %}

{% content-ref url="query-tokens-in-a-collection.md" %}
[query-tokens-in-a-collection.md](query-tokens-in-a-collection.md)
{% endcontent-ref %}

{% content-ref url="get-the-count-of-tokens-in-a-collection.md" %}
[get-the-count-of-tokens-in-a-collection.md](get-the-count-of-tokens-in-a-collection.md)
{% endcontent-ref %}

{% content-ref url="get-single-token-info.md" %}
[get-single-token-info.md](get-single-token-info.md)
{% endcontent-ref %}

{% content-ref url="gets-activities-of-a-token.md" %}
[gets-activities-of-a-token.md](gets-activities-of-a-token.md)
{% endcontent-ref %}

{% content-ref url="upload-order.md" %}
[upload-order.md](upload-order.md)
{% endcontent-ref %}
