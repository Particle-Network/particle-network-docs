---
description: User session key to sign automatically for the smart account
---

# Session Key

## What are session keys?[​](https://docs.zerodev.app/use-wallets/use-session-keys#what-are-session-keys) <a href="#what-are-session-keys" id="what-are-session-keys"></a>

In the world of cryptography, the private key represents everything, and disclosing the private key is equivalent to revealing our entirety. Therefore, in practical usage, every operation in a project requires user authorization for signing. While this approach is very cautious, it may not be convenient for operations that are frequent but have relatively low security requirements.

So, we need to  scope a key so that it can only send a limited set of transactions, under specific conditions, within a specific time window. We call these scoped keys "session keys."

## What can you use session keys for?[​](https://docs.zerodev.app/use-wallets/use-session-keys#what-can-you-use-session-keys-for) <a href="#what-can-you-use-session-keys-for" id="what-can-you-use-session-keys-for"></a>

There are a few core use cases of session keys:

* **Skipping request signing permission from the user**: After obtaining the user's authorization, you can use the public and private keys you created temporarily to sign the UserOp without having to request signing permission from the user. This is useful in some high-frequency operation scenarios (such as Games)
* **Delegating transactions**: The object of authorization can also be other projects, such as licensing some trading platforms to buy NFT prices for you when the price falls below a certain threshold

## Get Started

1. Use our high-level RPCs to construct and use the session key\
   [account-abstraction-rpc.md](../../rpc/account-abstraction-rpc.md "mention")
2. Use our SDKs to construct and use the session key\
   [sdks](../../sdks/ "mention")

## Demo

1. Learn how to use session key to mint nft without signing

{% content-ref url="biconomy-account.md" %}
[biconomy-account.md](biconomy-account.md)
{% endcontent-ref %}

