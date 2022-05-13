# Enhanced RPC

## Code Sample

```typescript
import axios from "axios";

(async () => {
  const response = await axios.post(
    "https://api.particle.network/solana/rpc",
    {
      chainId: 101,
      jsonrpc: "2.0",
      id: "0",
      method: "enhancedGetTokensAndNfts",
      params: ["GW9aT6HgEJbDE4wJnKn27qHQw6z4k8aMhYMxnUBmsiy3"],
    },
    {
      auth: {
        username: "Your Project Id",
        password: "Your Project Server Key",
      },
    }
  );
  // Use the data from the response
  console.log(response);
})();
```
