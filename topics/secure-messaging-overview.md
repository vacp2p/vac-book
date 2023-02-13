
# Secure Messaging (SeM) Overview

> *Note*: This page is still WiP.
This page will be published either as a page on `vac.dev`,
or alternatively on `sem.vac.dev`.

Vac's Secure Messaging (SeM) project researches and designs
modular p2p protocols for scaling, secure, private, anonymous, and censorship-resistant communications.
The main focus is [Waku](https://waku.org), the communication layer for Web3.

The following tracks are part of SeM.

### Secure Scaling

[*Secure Scaling Roadmap*](https://github.com/vacp2p/research/issues/154)

The *secure scaling* track is concerned with scaling Waku protocols in a secure and efficient way.
This includes DoS protection and resilience, as well as making sure that privacy and anonymity properties are not violated.

### Restricted Run

[*Restricted Run Roadmap*](https://github.com/vacp2p/research/issues/153)

Waku's [adaptive node](https://rfc.vac.dev/spec/30/) concept allows nodes of (almost) any resource profile to participate in Waku networks.
Nodes with more resources contribute the network, incentivized by various means addressed in the [protocol incentivization track](#protocol_incentivization).
Nodes with restricted resources mainly consume services.

The *restricted run* track researches and develops protocols and protocol extensions for resource restricted nodes.
The restricted run track covers the need of restricted devices in areas covered by the other tracks (e.g. secure scaling, anonymity, discovery),
and can be seen as orthogonal to these tracks.

The complexity that makes protocols like [11/WAKU2-RELAY](https://rfc.vac.dev/spec/11/)
too resource intense for some nodes also provides (depending on the specific protocol)
desirable privacy, anonymity, resilience, and latency properties.
A special challenge in the restricted run track stems from compensating these trade-offs as well as possible.
We aim to offer restricted nodes protocols that first and foremost allow network participation at all,
but also have desirable properties beyond that.

Examples for restricted run protocols are

* [12/WAKU2-FILTER](https://rfc.vac.dev/spec/12/)
* [19/WAKU2-LIGHTPUSH](https://rfc.vac.dev/spec/19/)
* [34/WAKU2-PEER-EXCHANGE](https://rfc.vac.dev/spec/34/)

*Restricted run* also covers the needs of restricted nodes beyond protocol design, e.g. NAT traversal.

### Discovery

[*Discovery Roadmap*](https://github.com/vacp2p/research/issues/116)

In order to build a P2P network,
participating nodes first have to discover peers within this network.
Ambient peer discovery allows nodes to find peers, making it an integral part of any decentralized application.
Our [research log post on Ambient Peer Discovery](https://vac.dev/wakuv2-apd) gives more background on this research area.

The *discovery* track aims to

* improve our existing discovery methods in terms of privacy, anonymity, resilience, and network efficiency
* research and design new discovery methods, as we want the best possible connectivity for Waku nodes

### Application Protocols

[*Application Protocols Roadmap*](https://github.com/vacp2p/research/issues/165)

### Privacy & Anonymity

[*Anonymity Roadmap*](https://github.com/vacp2p/research/issues/107)

One of Waku's main design goals is being privacy and anonymity preserving.
Waku v2 is modular and was designed with pluggable privacy/anonymity in mind.

The *Anonymity* track analyses current Waku v2 protocols with respect to their privacy/anonymity guarantees,
as well as plans to evaluate, specify, and implement new protocols that will enhance Waku v2 privacy/anonymity.

Our [research log post on Waku Relay Anonymity](https://vac.dev/wakuv2-apd) provides more background.
Adversarial models we consider are listed in [45/WAKU2-ADVERSARIAL-MODELS](https://rfc.vac.dev/spec/45/).

### Conversational Security

[*Conversational Security Roadmap*](https://github.com/vacp2p/research/issues/169)

### RLNP2P

[*RLNP2P Roadmap*](https://github.com/vacp2p/research/issues/179)

See intro here: [rlnp2p.vac.dev](https://rlnp2p.vac.dev/)

The *protocol incentivization* track uses results from *rlnp2p*.

### Protocol Incentivization

[*Protocol Incentivization Roadmap*](https://github.com/vacp2p/research/issues/171)

The *protocol incentivization* track covers research on incentives for nodes to offer specific services and/or resources.

Part of this is research on service credentials.
The goal is allowing service consumers to purchase *service credentials* (or tokens) that can be used to obtain services from Waku nodes.
Service providers will be able to claim funds associated with received credentials.

### Data Synchronization

[*Data Synchronization Roadmap*](https://github.com/vacp2p/research/issues/170)

The SeM *data synchronization* track covers research of secure and efficient asynchronous data synchronization between nodes.
It complements live messaging via [11/WAKU2-RELAY](11/WAKU2-RELAY).

### Censorship Resistance

Future Work.
