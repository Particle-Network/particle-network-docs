---
description: Native Support for IPFS!
---

# IPFS API

### Upload Files

[IPFS ](https://ipfs.io)is supported out of the box when using Particle Network.

You can upload files with the API _/ipfs/upload_ (max file size 100 MB).



Request Example

{% tabs %}
{% tab title="Node.js" %}
```typescript
const formData = require('form-data');
const fs = require('fs');
const axios = require('axios');

const IPFS_URL = 'https://api.particle.network/ipfs/upload';

(async () => {
    const filePath = 'Your file path';

    const form = new formData();
    form.append('file', fs.createReadStream(filePath));

    let res = await axios.post(IPFS_URL, form, {
        headers: form.getHeaders(),
        auth: {
            username: 'Your Project Id',
            password: 'Your Project Server Key',
        },
    });

    console.log(res.data);
})();
```
{% endtab %}
{% endtabs %}

Response Example

```typescript
{
    // ipfs cid
    "cid": "QmY21xP5qjz866TBovLH2Px5fLEyfU1uBg69BfDpdQu4KJ",
    // your file url
    "url": "https://gateway.pinata.cloud/ipfs/QmY21xP5qjz866TBovLH2Px5fLEyfU1uBg69BfDpdQu4KJ"
}
```

