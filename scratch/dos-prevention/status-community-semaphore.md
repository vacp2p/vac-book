# Community DoS protection with Semaphore

## Background

This proposal takes inspiration from [Alvaro's DoS protection scheme, i.e splitting app-level and network-level validation](https://github.com/vacp2p/research/issues/164#issuecomment-1418967178)

waku-rln-relay[^1][^2] in its current state, is not production ready. There are open problems being worked on, including, but not limited to, robustness[^3], circuit security[^4] as well as performance[^5].

After these open problems are addressed, we may look at using RLN for community DoS protection.

RLN is based on Semaphore[^6], with the added functionality of slashing. However, slashing is very rarely required in the event of spamming from a *trusted* set of peers. 

> *trusted* implies that the peer has been verified (out of band) before being added to a set of trusted peers.

Semaphore's latest version[^7] has been audited and can be considered safe to use in production.


## Assumptions

- There is no need to slash a community member spamming messages
- We simply route the messages if the member belongs to the Semaphore group, and drop them if not.

## Optional 

An optional pre-requisite for this solution is to use the Waku Message UID[^8], and updating the behaviour of dissemination of the `COMMUNITY_DESCRIPTION` message.

- Currently, the `COMMUNITY_DESCRIPTION` message is sent at fixed intervals *and* whenever the metadata of the community has changed
- When a new member is added to the community, their public key is appended to a list of members' public keys, and broadcasted with the `COMMUNITY_DESCRIPTION` message

This change proposes that the `COMMUNITY_DESCRIPTION` that is broadcasted at fixed intervals, merely carries a reference to the change in the community metadata, which was broadcasted when the change happened, i.e a MUID of the message that includes the change.

Sending the MUID allows reduced message sizes, and can leverage waku store lookups for old messages.

## Working

This method requires all members to keep a local copy of a sparse merkle tree containing the commitments (sent out of band to the community owner) of the members of the community. Having the MUID of the latest tree state broadcasted by the community owner is very useful here.

Whenever the member wishes to send a message, they attach a Semaphore proof to it (increasing message size by *TODO*). 

Messages without proofs attached/invalid proofs can be dropped, thereby reducing their propagation through the network.

## Conclusion

RLN and Semaphore-based DoS protection can be used in tandem, RLN for channels with slow-mode enabled, and Semaphore for all other channels.

## References

[^1]: https://rfc.vac.dev/spec/17/
[^2]: https://rfc.vac.dev/spec/32/
[^3]: https://github.com/waku-org/nwaku/issues/1501
[^4]: https://github.com/Rate-Limiting-Nullifier/rln-circuits/pull/7
[^5]: https://github.com/waku-org/nwaku/issues/1501
[^6]: https://semaphore.appliedzkp.org/
[^7]: https://github.com/semaphore-protocol/semaphore/releases/tag/v3.0.0
[^8]: https://github.com/waku-org/pm/issues/9