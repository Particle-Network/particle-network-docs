# Manage Assets

As we all know, the blockchain assets are digital, owned solely by user, and are immediately transferable at any time to any person.&#x20;

But in some cases, we need the ability to control user's assets in the project for safety or operational needs.

Now you can have the ability, we'll detail the process of how to manage user's assets in your project.

### Adding a Project Contract

1. Select the project you'd like to add a project contact to and enter its project overview page
2. Select **"Add Project Contract"**
3. Input relevant information to the project contract:
   * Contract name: this is only displayed within the dashboard, used to distinguish between different contracts for easy management.
   * Contract address: you assets contract address
4. Save your contract information

### Verifying a Project Contract

1. Choose the contract you'd like in project contracts and click **"To Verify"**&#x20;
2. Input relevant information to verify the contract:
   * Signature: this is come from the contract creator sign a specific string, used to verify the contract is belong to you
   * Create transaction hash: this is only need by EVM-Chains, used to verify contract creator information
3. Click **"Verify this contract"**

Here are the details for get the signature:

String to sign: `PARTICLE NETWORK ${YOUR_PROJECT_ID} ${YOUR_CONTRACT_ADDRESS}`

Example for generate signature:

```javascript
import { ecsign, sha256, toRpcSig } from "ethereumjs-util";

const strToSign = `PARTICLE NETWORK ${YOUR_PROJECT_ID} ${YOUR_CONTRACT_ADDRESS}`;
const msgHash = sha256(Buffer.from(strToSign));
const sign = ecsign(msgHash, YOUR_PRIVATE_KEY);
console.log("signature: ", toRpcSig(sign.v, sign.r, sign.s));
```

### Locking User's Assets in Your Project

👉[#lockuserwallet](../auth-service/sdks/server-api.md#lockuserwallet "mention")&#x20;

### Unlocking User's Assets in Your Project

👉[#unlockuserwallet](../auth-service/sdks/server-api.md#unlockuserwallet "mention")

Or you can unlock in the dashboard do as follows:

1. &#x20;Select **"Locking"** menu in assets control
2. Chose the user by address you'd like in the locking list and click **"Unlocked"**

