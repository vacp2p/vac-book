# Waku Network and Waku Products

> *Note*: This is just an off-the-cuff opinion draft on how Waku products could develop in the future.
(Almost) everything written here should be read as prefaced with "Imo, we could ..." and such.

Waku is offered as a set of products with different trade-offs to batteries-included-ness and customizability.
These products comprise:

* specifications
* Waku Implementations
* Dedicated Shards
* Waku Network
    - main-net with focus on scaling
    - second net with focus on anonymity

*We recommend projects to use the Waku Network.*

## Specifications

[Specifications](https://rfc.vac.dev/spec/10/) allow projects to work on their own Waku implementations.
Following the specifications guarantees interoperability with other implementations.
(These custom implementations can be used in any of the following categories.

## Waku Implementations

Waku offers several implementations, each of which can be configured to use project specific infrastructure; e.g. a completely separate project specific P2P networks, with a separate project-specific discovery network.

> *Note*: These implementations can/will be used for the following approaches with more batteries included, too.
The "Implementation" product category just shows that projects can use the Waku Implementations with completely segregated project-specific networks.

## Dedicated Shards

Dedicated Shards allow projects to use separate project specific Relay networks (shards in a shard cluster).
This allows defining separate/custom

* relay based DoS mitigation
* content topic to shard mapping
* incentivization
* project exclusive infrastructure (if properly protected)

while still being able to use the Discovery methods provided by the Waku discovery network.

> *Note*: This is what Status will be using for the MVP.

## Waku Network

"Waku Network" is the main Waku network, similar to the Ethereum network.
Relay based DoS mitigation as well as incentivization are governed by Waku.
Still, project/apps may employ different value extraction models.
Content topics are mapped using [automatic sharding](https://rfc.vac.dev/spec/51/#automatic-sharding).
(Both static and automatic discovery use the same discovery method;
automatic sharding will use a set of shard clusters into which it automatically maps content topics.)

Nodes are not dedicated to projects, but are part of (dynamically allocated parts of) the whole Waku Network;
this allows for better k-anonymity.
(This is a future goal, as various research questions are still open.)

Waku does not own or control the "Waku Network";
each project using it contributes.

### Focus on Scaling vs Anonymity

While having a scaling anonymity preserving Waku Network is the long term goal,
this is very challenging to achieve.
Protecting anonymity makes incentivization and DoS protection much more difficult.

For this reason, it might make sense to have two Waku Networks; one with focus on scaling (the main net for now),
and one with a focus on sender anonymity.
These networks would be mapped into different shard clusters.

The scaling network, too, would offer privacy properties:

* transport security
* no single entity controlling the infra
* receiver anonymity (k-anonymity)
* pseudonymity
* e2e encryption (confidentiality) can be employ by app protocols; they can also use app protocols like 1:1 chat provided by Waku in the future
* ...

It might make sense to even sign messages in this network or associate messages/PeerIDs with on-chain addresses.
This would cost anonymity, but would allow for much better scaling in the mid-term.
Apps/projects could choose which of the two Waku networks they want to use.
The scaling network could become production ready in the mid-term and apps could simply use it,
without having to worry about DoS etc.
(The terms would have to clearly state that (strong) *anonymity* is not a property of this network.)

