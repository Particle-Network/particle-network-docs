---
description: GET /chains/{chainId}/contractAddress/{contractAddress}/tokens/count
---

# 🔥 Get the count of tokens in a Collection

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
        }
    }
    ```

**Results:**

* `<object>` - a object contains count

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
const url = `${baseUrl}/chains/${chainId}/contractAddress/${contractAddress}/tokens/count`;

(async () => {
    const response = await axios.get(url, {
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
                }
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
