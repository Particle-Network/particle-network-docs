# Biconomy Account

The basic process is as follows:

1. Authorize Session
   1. Create a Session containing the following information:
      1. `sessionKeyData`: Metadata for the validation contract, which includes a temporary public key address and some other parameters. This needs to be registered in the SmartAccount. During the verification process, SmartAccount will pass the specified SessionKeyData to the validation contract for reading and usage.
      2. `sessionValidationModule`: A custom validation contract that replaces the validateUserOp functionality. Its responsibility is to verify whether UserOp is legitimate, such as being signed by an authorized Session Signer and for a specified purpose.
   2. Register the Session on the blockchain.
2. Subsequent UserOps for specific purposes can then be directly signed using the Session Signer.

<figure><img src="../../../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

### Custom Validation Contract

The interface is implemented as follows

```typescript
// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

import {UserOperation} from "@account-abstraction/contracts/interfaces/UserOperation.sol";

abstract contract ISessionValidationModule {
    // execute(address,uint256,bytes)
    bytes4 public constant EXECUTE_SELECTOR = 0xb61d27f6;
    // execute_ncC(address,uint256,bytes)
    bytes4 public constant EXECUTE_OPTIMIZED_SELECTOR = 0x0000189a;

    /**
     * @dev validates if the _op (UserOperation) matches the SessionKey permissions
     * and that _op has been signed by this SessionKey
     * @param _op User Operation to be validated.
     * @param _userOpHash Hash of the User Operation to be validated.
     * @param _sessionKeyData SessionKey data, that describes sessionKey permissions
     * @param _sessionKeySignature Signature over the the _userOpHash.
     * @return true if the _op is valid, false otherwise.
     */
    function validateSessionUserOp(
        UserOperation calldata _op,
        bytes32 _userOpHash,
        bytes calldata _sessionKeyData,
        bytes calldata _sessionKeySignature
    ) external virtual returns (bool);
}
```

### Example: mint one NFT for  a specific address every time

1. Prepare an ERC 1155 NFT contract
   1. https://mumbai.polygonscan.com/address/0x909E30bdBCb728131E3F8d17150eaE740C904649
2. Define the `sessionKeyData` data structure

```typescript
Abi.encode(
    [
        "address", // session signer address
        "address", // recipient, which can receive the nft
        "uint256"  // specified nft token id
    ]
)
```

3. Prepare a custom validation contract
   1. https://mumbai.polygonscan.com/address/0x8E09744b738e9Fec4A4df7Ab5621f1857F6Fa175#code
4. Register the session

```typescript
import Axios from "axios";
import { Wallet, utils } from "ethers";

const chainId = 80001;
const PARTICLE_RPC = `https://rpc.particle.network/evm-chain/public?chainId=${chainId}`;
const scanUrl = 'https://mumbai.polygonscan.com';
const nft1155ContractAddress = "0x909E30bdBCb728131E3F8d17150eaE740C904649";
const customValidationContractAddress = "0x8E09744b738e9Fec4A4df7Ab5621f1857F6Fa175";

const mainSigner = Wallet.createRandom();
const sessionSigner = Wallet.createRandom();
const smartAccount = { name: "BICONOMY", version: "2.0.0", ownerAddress: mainSigner.address };

(async () => {
    let sessions: any[] = [
        {
            validUntil: 0, // 0 means no expiry
            validAfter: 0, // 0 means no start time
            sessionValidationModule: customValidationContractAddress,
            // you can use `sessionKeyData` to replace `sessionKeyDataInAbi`
            // sessionKeyData: abi.encode(...)
            sessionKeyDataInAbi: [
                ["address", "address", "uint256"],
                [
                    sessionSigner.address, // session signer address
                    sessionSigner.address, // receiver address
                    1, // nft token id
                ],
            ],
        },
    ];

    const resCreateSessions = await Axios.post(PARTICLE_RPC, {
        method: "particle_aa_createSessions",
        params: [smartAccount, sessions],
    });

    // we use gasless mode
    const userOp = resCreateSessions.data.result.verifyingPaymasterGasless.userOp;
    const userOpHash = resCreateSessions.data.result.verifyingPaymasterGasless.userOpHash;
    sessions = resCreateSessions.data.result.sessions; // the sessions you need to store locally
    userOp.signature = await mainSigner.signMessage(utils.arrayify(userOpHash));

    const resSendUserOp = await Axios.post(PARTICLE_RPC, {
        method: "particle_aa_sendUserOp",
        params: [smartAccount, userOp],
    });

    console.log(`${scanUrl}/tx/${resSendUserOp.data}`);
})();
```

5. Use session signer to sign the userOps follows

```typescript
const resValidateSession = await Axios.post(PARTICLE_RPC, {
    method: "particle_aa_validateSession",
    params: [
        smartAccount,
        {
            sessions, // all sessions to generate proof
            targetSession: sessions[0], // which session to check if valid
        },
    ],
});

const isValidSession = resValidateSession.data.result;
if (!isValidSession) {
    throw new Error("Session is not valid");
}

const erc1155Contract = new Contract(nft1155ContractAddress, ["function mintTo(address, uint256, uint256) public"], provider);
const tx = await erc1155Contract.populateTransaction.mintTo(sessionSigner.address, 1, 1);

const resGetFeeQuotes = await Axios.post(PARTICLE_RPC, {
    method: "particle_aa_getFeeQuotes",
    params: [smartAccount, [tx]],
});

const userOp = resGetFeeQuotes.data.result.verifyingPaymasterGasless.userOp;
const userOpHash = resGetFeeQuotes.data.result.verifyingPaymasterGasless.userOpHash;
userOp.signature = await sessionSigner.signMessage(utils.arrayify(userOpHash));

const resSendUserOp = await Axios.post(PARTICLE_RPC, {
    method: "particle_aa_sendUserOp",
    params: [
        smartAccount,
        userOp,
        {
            sessions, // all sessions to generate proof
            targetSession: sessions[0], // which session to use in this userOp
        },
    ],
});

console.log(`${scanUrl}/tx/${resSendUserOp.data.result}`);
```

### Management and Use of Session&#x20;

1. Since only the hash value of the session collection is stored on the chain, the specific data of sessions needs to be saved off-chain, usually by the client itself. And because each update chain of sessions is in the form of overwriting the original hash, each time a session is registered, all sessions need to be passed in.&#x20;
2. Generally speaking, for the client, every time a session is updated, only all the sessions that are within the validity period need to be passed
