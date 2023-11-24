# Session Key

## What are session keys?[​](https://docs.zerodev.app/use-wallets/use-session-keys#what-are-session-keys) <a href="#what-are-session-keys" id="what-are-session-keys"></a>

In the world of cryptography, the private key represents everything, and disclosing the private key is equivalent to revealing our entirety. Therefore, in practical usage, every operation in a project requires user authorization for signing. While this approach is very cautious, it may not be convenient for operations that are frequent but have relatively low security requirements.

So, we need to  scope a key so that it can only send a limited set of transactions, under specific conditions, within a specific time window. We call these scoped keys "session keys."

## What can you use session keys for?[​](https://docs.zerodev.app/use-wallets/use-session-keys#what-can-you-use-session-keys-for) <a href="#what-can-you-use-session-keys-for" id="what-can-you-use-session-keys-for"></a>

There are a few core use cases of session keys:

* **Skipping confirmations**: if you are building a highly interactive application, you might wish that your users do not have to manually confirm every single transaction. Instead, you can create a session key for your user's current "session." You would scope the key so it can only send the transactions that are required by your application, AND the key expires after the current session. Now, your user can interact with your app with the session key, without using their master key which would require confirming every transaction.
* **Delegating transactions**: normally, transactions need to be actively initiated by the wallet owner. However, sometimes the best UX is enabled when transactions can be "automated." For example, if you are building a lending protocol, you might want to build a feature where your user can automatically close their position if it's in danger of liquidation. Here, you could create a session key that allows for closing a position ONLY IF the position is indeed close to liquidation. And the session key can be shared with a "watcher" (possibly your own server) that would send the transaction for the user when the condition occurs.

{% content-ref url="biconomy-account.md" %}
[biconomy-account.md](biconomy-account.md)
{% endcontent-ref %}

