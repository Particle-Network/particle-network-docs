---
description: The particle solana standard transaction struct.
---

# Transaction Struct

Some APIs will return transactions to be signed, such as Mint NFT, Transfer Token, etc.

Transactions in the API are generated using [@solana/web3.js](https://github.com/solana-labs/solana-web3.js).

### Data Struct

```typescript
export interface ISolanaTransaction {
    // whether the transaction was partially signed
    isPartialSigned: boolean;
    // base58.encode(transaction.serialize({ requireAllSignatures: false }))
    serialized: string; 
    // signers generated in the API
    // if an error occurs during the execution of the transaction, it can be used when retrying
    // base58.encode(Keypair.secretkey)
    signers: string[]; 
}
```

### Example

```typescript
{
    "chainId": 103,
    "jsonrpc": "2.0",
    "id": 0,
    "result": {
        "mint": "5DTYtoLtdWnUjtkf9abuyPDKA1K3uizjNZBj2saWdBHv",
        "mintAssociatedTokenAccount": "Hmi2gfLhzzveeT58yWUXDVcfLGcrgGzMfmQ3LPFoe3J3",
        "transaction": {
            "isPartialSign": true,
            "serialized": "PFPXCr7kuoiiw3y55ACWKYeYdCUsXw5fnQ384LuX4c8djRtgVKmT3hdNtEhMuw5YQooVLSDA33ri1exvX2mQ7GEQ2LUkHkaY7QeYL9UAcFXyAYqPL88xeFrADWzZ4H4XuAemCYwoTRNwogRNfonuZSeq8CVSzymYHZ36qVtuT28oiohoEFSQGDt9q8NzoT9QebFWojJ3Zfp4Twef1h5QeK3Y9ffamZLzqWoGHmZsJHDRhikA7HkMZXNiW5qG87xpg9mCwLQZFWBLXwhGDbiyi2YC8DvoGniieem9ftPS9m7e4ufZfZktn5wdTUnGSXrRU7jpbmRqR8Eutszig8Y1yewGYESjCWKjWFCk3fD1uzbGTEJyZjQkoUNqizREEBBUq11FeoSFreKWX3xUiEvUe7BNYBAUJKP9CvLJe7ZiD4f3voWMEjorkB4qKWXXFct7o7Tm3fJH5LAucvwpimnFkbkKA6Nou9WBUV6upUsM67u1hgKiwZj2wMxhd9t9nu3by2rHhhScjrGg3EnhyH8vpq6mf645bqubjfEDodknvy1zaswVuJQFUh8ZaCLyB3gmRB8c9xwwZHwcgfV4bs5qVMKxK4PQxR3ArYVrLLq2yXRvVexF1YuF8GQSuZ6LyNggwVBK2t8trxjtepAWNpug8B3zmwxquGPfunKtwRtNvJXkrp23TkDrCHtZSnQBGo6d5zXD7bG4sDq85H7Y1FwtEeDVQTxhcvLG46ZVBkkYKxegGBTcjLmLjk1LyqqoF6QSVmZcGHD56FPTjHZaLnzXzzNozqx9ApytuZkQcwDWV9UWsWAwWbvGtGoUWkDcDfVEKCv1BDGYcqGuSp27Fmz1Y1pVdQ4BV3sxdy3Bp4yzNRaXbJpSnASxLmn3ixSs4g1Q6c6SHoYqWnXgEmgM8VL4JpCehqmdGDCvwzi7bE9dHYZ8DLL5aCA2nJmC4L89PMBVRxxLAc3Acvn9jRhBieRoCfxMrDPjjbmx47tUN8s93e9xvkR3ssX4Pb1SyN6aKqfGH1vZz2azqdSUTGaVT5ZC7Zh7jnXSzWXQpjkZbGeKarxB6HjCJR4DmfZD7q74K7ZmbsNS9TKqMmjiffExvCRJKzmG2afjSTTnET5c2NWezZby4WbnCo",
            "signers": [
                "5p64ZhpEQYGmzspkV6Lc1AMhdvemmPuHKZMnEc4knhxbUyUPqWvQvwfqcsuB2zNQbBFAvqCzbANuwtNnhKN8CC5J"
            ]
        }
    }
}
```

### Sign and execute the transaction

You can sign the transaction yourself by using  [@solana/web3.js](https://github.com/solana-labs/solana-web3.js), or you can use the [Particle Auth Service](https://docs.particle.network/auth-service/introduction) to sign the transaction.

```typescript
const axios = require('axios');
const bs58 = require('bs58');
const { Keypair, Transaction } = require('@solana/web3.js');

const SOLANA_RPC_URL = 'https://api.particle.network/solana/rpc';

// generate or set a keypair
const keypair = Keypair.fromSecretKey(Buffer.from([]));

const auth = {
    username: 'Your Project Id',
    password: 'Your Project Server Key',
};

(async () => {
    const responseMintNFT = await axios.post(SOLANA_RPC_URL, {
        chainId: 103,
        method: 'NFT_mint',
        params: [keypair.publicKey.toBase58(), {
            metadata_uri: 'https://gateway.pinata.cloud/ipfs/QmVJ8tG8nuqCk8QeZmBWkQxJKhSx55kprw8i18LiWXU9f8',
        }],
    }, { auth });

    console.log(responseMintNFT.data);

    const transaction = Transaction.from(bs58.decode(responseMintNFT.data.result.transaction.serialized));
    transaction.partialSign(keypair);

    // send and confirm transaction
    const responseConfirmTransaction = await axios.post(SOLANA_RPC_URL, {
        chainId: 103,
        method: 'enhancedSendAndConfirmRawTransaction',
        params: [bs58.encode(transaction.serialize())],
    }, { auth });

    // return the transaction signature
    console.log(responseConfirmTransaction.data);
})();
```
