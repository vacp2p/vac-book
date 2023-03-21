# Waku pubsub topic sharding

## Context
The following document provides an overview of the Waku pubsub topic sharding method, which is based on the [35/WAKU2-NOISE](https://rfc.vac.dev/spec/35/) and [23/WAKU2-TOPICS](https://rfc.vac.dev/spec/23/#23waku2-topics) RFCs.

## Method
The Waku pubsub topic sharding method is based on the use of a shared secret key derived from a Diffie-Hellman key exchange, and a deterministic hash function. 
The method is described as follows:

1. The two parties, Alice and Bob, establish a shared secret key using a Diffie-Hellman key exchange.

2. The shared secret key is used as an input to a deterministic hash function, such as SHA256, to generate a new topic for the next message.

    $$
    pubsub\_topic = sha256(shared\_sk  \ mod \  privacy\_parameter)
    $$

3. For each subsequent message, the shared secret key (which has been recomputed) is used as the input to the hash function again to generate a new topic.

4. (optional) To ensure the topic is unique, a nonce is concatenated to the shared secret key before hashing.

    $$
    pubsub\_topic = sha256((shared\_sk  \ mod \  privacy\_parameter) ⌢ nonce)
    $$

5. (optional) To ensure the topic is different for each message, a counter is concatenated to the shared secret key before hashing.

    $$
    pubsub\_topic = sha256((shared\_sk ⌢ ctr)  \ mod \  privacy\_parameter)
    $$
    
6. (optional) To reduce computational overhead of calculating a new pubsub topic for each message, a time window can be agreed upon between Alice and Bob. 
  For example, a 24hr window can be negotiated if it is within the constraints that both Alice and Bob agree to.

    Therefore, the pubsub topic is calculated in the following manner
    $$
    pubsub\_topic = sha256((shared\_sk⌢ timestamp)  \ mod \  privacy\_parameter) 
    $$
    Note that this approach would require both Alice and Bob to keep track of the shared secret key used at the start of the negotiation to derive the next pubsub topic. 
    This cache can be erased when the negotiation process restarts at the end of the window.


The `privacy_parameter` can be set by the peers, and must be agreed on beforehand between Alice and Bob. 
The lower the `privacy_parameter` is, the set of values for the pubsub topic would be fewer and it offers k-anonymity based privacy benefits. 
The higher the `privacy_parameter` is, the set of values for the pubsub topic 'B' would be larger, therefore, performance is increased at the cost of privacy.

It's important to note that the shared secret key changes with each message sent, as per Noise processing rules. 
This allows updates to the shared secret key by hashing the result of an ephemeral-ephemeral Diffie-Hellman exchange every 1-RTT communication. Therefore, even if privacy guarantees are lost with one message, they can be regained with the next message, if Alice signals to Bob (or vice-versa), to change the privacy_parameter

One can argue that this approach may lead to failure in forming a mesh, but with the assumption that: 
- the number of community-run nodes are high, and they participate in meshing
- peers are incentivised to relay messages on a pubsub topic (incentivisation is yet to be solved)

If any of these assumptions are invalid, the peers can resort to using a lower `privacy_parameter` which would result in a commonly used pubsub topic, and piggyback on other peers that have a vested interest in that pubsub topic.

## Security Considerations
The Waku pubsub topic sharding method has several potential security considerations that must be taken into account, including:

1. The security of the shared secret key depends on the security of the Diffie-Hellman key exchange, which can be vulnerable to various attacks if not implemented correctly.

2. The privacy benefits of k-anonymity depend on the value of the privacy parameter. A low value of the privacy parameter may provide stronger privacy guarantees, but at the cost of lower performance. A high value of the privacy parameter may provide better performance, but at the cost of weaker privacy guarantees.

3. An attacker who is able to obtain the shared secret key or the privacy parameter may be able to determine the topic of a message and potentially intercept it.

4. The technique of updating the shared secret key with each message can be useful in protecting against eavesdropping, but it also requires additional computation and communication overhead.

5. The technique of updating the shared secret key with each message also requires a secure signaling mechanism to signal the privacy parameter between Alice and Bob.

Overall, it is important to carefully consider the trade-offs between performance and privacy when implementing the Waku pubsub topic sharding method, and to ensure that the key exchange and signaling mechanisms are implemented securely.

## Future research

- Peer incentivisation
- Check if this model can apply to [5/SECURE-TRANSPORT](https://specs.status.im/spec/5)'s cryptography

## Appendix A - Multicast group chats

In the context of Status Communities, where single ratchet encryption is used, the nonce used could be the channel name, thereby sharding communities at the channel level.

$$
pubsub\_topic = sha256((ratchet\_key ⌢ channel\_name)  \ mod \  privacy\_parameter) 
$$


One benefit of using this method, is that all community participants are aware of this, and can derive a set of pubsub_topics every time there is a key change (when a member is added or removed from a community).

## Appendix B - Message reconciliation and ordering

A method for reconciling and ordering messages across different pubsub topics while preserving privacy is as follows:

The sender can include a unique, monotonically increasing sequence number for each message. 
This allows the receiver to order the messages based on the sequence number, regardless of which pubsub topic the messages were sent on.

The fault-tolerant characteristic of the underlying store protocol is out of scope for this spec.
