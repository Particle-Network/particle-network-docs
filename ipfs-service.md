---
description: Native Support for IPFS!
---

# IPFS API

[IPFS ](https://ipfs.io)is supported out of the box when using Particle Network.

You can upload files with the API _ **/ipfs/upload**_ to upload file (max file size 100 MB), and use _**/ipfs/upload\_json**_ to upload json.

### Upload File

**Request Example**

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

**Response Example**

```typescript
{
    // v1 cid: @see https://docs.ipfs.io/concepts/content-addressing/#identifier-formats
    "cid": "bafybeidhaeimld7itiwr4v4jpxiscbkrxioapmawty7mhucp5kcdywgcvi",
    // Preferred url
    "url": "https://bafybeidhaeimld7itiwr4v4jpxiscbkrxioapmawty7mhucp5kcdywgcvi.ipfs.nftstorage.link",
    // Other working urls: @see https://ipfs.github.io/public-gateway-checker/
    "urls": [
        "https://bafybeidhaeimld7itiwr4v4jpxiscbkrxioapmawty7mhucp5kcdywgcvi.ipfs.nftstorage.link",
        "https://bafybeidhaeimld7itiwr4v4jpxiscbkrxioapmawty7mhucp5kcdywgcvi.ipfs.dweb.link",
        "https://bafybeidhaeimld7itiwr4v4jpxiscbkrxioapmawty7mhucp5kcdywgcvi.ipfs.infura-ipfs.io",
        "https://bafybeidhaeimld7itiwr4v4jpxiscbkrxioapmawty7mhucp5kcdywgcvi.ipfs.4everland.io",
        "https://bafybeidhaeimld7itiwr4v4jpxiscbkrxioapmawty7mhucp5kcdywgcvi.ipfs.cf-ipfs.com",
        "https://bafybeidhaeimld7itiwr4v4jpxiscbkrxioapmawty7mhucp5kcdywgcvi.ipfs.ipfs-gateway.cloud",
        "https://bafybeidhaeimld7itiwr4v4jpxiscbkrxioapmawty7mhucp5kcdywgcvi.ipfs.storry.tv",
        "https://ipfs.io/ipfs/bafybeidhaeimld7itiwr4v4jpxiscbkrxioapmawty7mhucp5kcdywgcvi",
        "https://gateway.ipfs.io/ipfs/bafybeidhaeimld7itiwr4v4jpxiscbkrxioapmawty7mhucp5kcdywgcvi",
        "https://cloudflare-ipfs.com/ipfs/bafybeidhaeimld7itiwr4v4jpxiscbkrxioapmawty7mhucp5kcdywgcvi"
    ]
}
```

### Upload JSON

**Request Example**

{% tabs %}
{% tab title="Node.js" %}
```typescript
const axios = require('axios');

const IPFS_URL = 'https://api.particle.network/ipfs/upload_json';

(async () => {
    let res = await axios.post(IPFS_URL, {// json data}, {
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

**Response Example**

```typescript
{
    // v1 cid: @see https://docs.ipfs.io/concepts/content-addressing/#identifier-formats
    "cid": "bafybeiatchjoos436vysr7q3tna6ubd2xgseylsh4uo66swxualkzbnlnq",
    // Preferred url
    "url": "https://bafybeiatchjoos436vysr7q3tna6ubd2xgseylsh4uo66swxualkzbnlnq.ipfs.nftstorage.link",
    // Other working urls: @see https://ipfs.github.io/public-gateway-checker/
    "urls": [
        "https://bafybeiatchjoos436vysr7q3tna6ubd2xgseylsh4uo66swxualkzbnlnq.ipfs.nftstorage.link",
        "https://bafybeiatchjoos436vysr7q3tna6ubd2xgseylsh4uo66swxualkzbnlnq.ipfs.dweb.link",
        "https://bafybeiatchjoos436vysr7q3tna6ubd2xgseylsh4uo66swxualkzbnlnq.ipfs.infura-ipfs.io",
        "https://bafybeiatchjoos436vysr7q3tna6ubd2xgseylsh4uo66swxualkzbnlnq.ipfs.4everland.io",
        "https://bafybeiatchjoos436vysr7q3tna6ubd2xgseylsh4uo66swxualkzbnlnq.ipfs.cf-ipfs.com",
        "https://bafybeiatchjoos436vysr7q3tna6ubd2xgseylsh4uo66swxualkzbnlnq.ipfs.ipfs-gateway.cloud",
        "https://bafybeiatchjoos436vysr7q3tna6ubd2xgseylsh4uo66swxualkzbnlnq.ipfs.storry.tv",
        "https://ipfs.io/ipfs/bafybeiatchjoos436vysr7q3tna6ubd2xgseylsh4uo66swxualkzbnlnq",
        "https://gateway.ipfs.io/ipfs/bafybeiatchjoos436vysr7q3tna6ubd2xgseylsh4uo66swxualkzbnlnq",
        "https://cloudflare-ipfs.com/ipfs/bafybeiatchjoos436vysr7q3tna6ubd2xgseylsh4uo66swxualkzbnlnq"
    ]
}
```
