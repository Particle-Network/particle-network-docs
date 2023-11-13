---
description: Native Support for IPFS!
---

# IPFS API

[IPFS ](https://ipfs.io)is supported out of the box when using Particle Network.

You can upload files with the API _**/ipfs/upload**_ to upload file (max file size 100 MB), and use _**/ipfs/upload\_json**_ to upload json.

### Upload File

**Request Example**

{% tabs %}
{% tab title="Node.js" %}
```typescript
const formData = require('form-data');
const fs = require('fs');
const axios = require('axios');

const IPFS_URL = 'https://rpc.particle.network/ipfs/upload';

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
    // IPFS format address
    "ipfs": "ipfs://bafybeidhaeimld7itiwr4v4jpxiscbkrxioapmawty7mhucp5kcdywgcvi",
    // Preferred url with CDN cache for better performance
    "fastUrl": "https://ipfs.particle.network/bafybeidhaeimld7itiwr4v4jpxiscbkrxioapmawty7mhucp5kcdywgcvi",
    // Preferred url
    "ipfsUrl": "https://bafybeidhaeimld7itiwr4v4jpxiscbkrxioapmawty7mhucp5kcdywgcvi.ipfs.nftstorage.link",
    // Other working urls: @see https://ipfs.github.io/public-gateway-checker/
    "ipfsUrls": [
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

const IPFS_URL = 'https://rpc.particle.network/ipfs/upload_json';

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
    // IPFS format address
    "ipfs": "ipfs://bafybeiatchjoos436vysr7q3tna6ubd2xgseylsh4uo66swxualkzbnlnq",
    // Preferred url with CDN cache for better performance
    "fastUrl": "https://ipfs.particle.network/bafybeiatchjoos436vysr7q3tna6ubd2xgseylsh4uo66swxualkzbnlnq",
    // Preferred url
    "ipfsUrl": "https://bafybeiatchjoos436vysr7q3tna6ubd2xgseylsh4uo66swxualkzbnlnq.ipfs.nftstorage.link",
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

### Gateway

Our IPFS gateway provides a read-only, HTTP-accessible interface to the Interplanetary File System (IPFS). It's **super fast** and **easy to use.**

#### How to use

{% hint style="info" %}
// format

**https://ipfs.particle.network/{cid}**



// For example

[https://ipfs.particle.network/QmUzWU3kjXHg8rKSSB8H7XUKbTPRhA2zcFQed2ytK1CPyS](https://ipfs.particle.network/QmUzWU3kjXHg8rKSSB8H7XUKbTPRhA2zcFQed2ytK1CPyS)[https://ipfs.particle.network/QmYDvPAXtiJg7s8JdRBSLWdgSphQdac8j1YuQNNxcGE1hg/1.png](https://ipfs.particle.network/QmYDvPAXtiJg7s8JdRBSLWdgSphQdac8j1YuQNNxcGE1hg/1.png)
{% endhint %}

In addition, we provide **cropping of image content** to help you optimize and speed up local content access.

{% hint style="info" %}
// format

**https://ipfs.particle.network/{cid}\_\_{width}x{height}**



// For example（Returns a 300x300 image）

[https://ipfs.particle.network/QmYDvPAXtiJg7s8JdRBSLWdgSphQdac8j1YuQNNxcGE1hg/1.png\_\_300x300](https://ipfs.particle.network/QmYDvPAXtiJg7s8JdRBSLWdgSphQdac8j1YuQNNxcGE1hg/1.png\_\_300x300)
{% endhint %}



