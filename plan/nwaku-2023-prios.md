
# 2023 nwaku priorities

> **Note:** I have identified 12 focus areas for nwaku to focus on in 2023 supporting our vision of Waku being used everywhere. Our ultimate goal is to have the nwaku client be a suitable way to achieve this. Under each focus area, I've extracted the first milestones where possible and if already defined. 12 focus areas are too much for a small team to handle, so I have condensed these points into three main priorities. It is important for the nwaku team to be mindful of taking on too much and to prioritise tasks that directly contribute to our vision for Waku. I've also considered the wider activities of Status, Vac, and Logos in the planning.

## Priorities TL;DR

1. Increase the reach of Waku and nwaku, by targeting node programs and supporting Status milestones.
2. Optimise nwaku for more environments and stabilise protocols.
3. Work with Vac Research and Logos Devops to build out capabilities to scale securely, provide service incentivisation and create an integrated network testing and simulation environment.

## Detailed focus areas

### 1. Target node programs for community-run nodes

Our main long-term goal as a team is to see Waku running everywhere and the nwaku client to be a suitable and reliable client in as many environments as possible. One way to reach out to more operators is to target node programs.

#### First milestones

- Milestone 1: Integrate nwaku into DappNode
- Milestone 2: Get to 100 user-run nodes, excluding Status clients.
- Milestone 3: Launch an incentivised node program (we could take inspiration here from [Kusama](https://thousand-validators.kusama.network/#/getting-started) or past [Status node program](https://our.status.im/status-node-program-launch/))

### 2. Support Status milestones

Improve collaboration here with Status to meet their ambitious milestones, especially for the Status Communities project.

#### First milestones

- Milestone 1: Status Mobile MVP: [Status Core Contributors use Status Mobile](https://github.com/waku-org/pm/issues/8)

### 3. Run nwaku in more places

We want to make it easuer to integrate nwaku into existing/new applications, allowing us to reach a larger potential user base. For example, adding C bindings, wrapping in Rust, etc.

#### First milestones

- Milestone 1: Expose C bindings for nwaku

### 4. Nwaku low-level performance optimisations

This could be related to the previous point. One of our goals with nwaku is to create a client that is suitable to run in resource-restricted environments. There are many parts of the existing client that can be significantly optimised in terms of resource (memory/CPU) and bandwidth usage.

We'd preferably get a new hire to focus on this.

### 5. Stable specification and implementation of restricted-run protocols

With `relay` reaching maturity and `store` well on its way, we want to focus on the main protocols required by resource-restricted devices, e.g. `filter`, `lightpush` and `waku-peer-exchange` to stabilise both the protocol specifications and implementations

#### First milestones

- Milestone 1: Store v3 protocol [stabilisation and beta implementation](https://github.com/vacp2p/rfc/issues/552)
- Milestone 2: Filter protocol [stabilisation and beta implementation](https://github.com/waku-org/pm/issues/5)

### 6. Secure scaling (collab with Vac Research)

Many applications using nwaku, such as Status, have ambitious scaling targets. The _Secure Messaging_ project in Vac Research is focusing on secure scaling strategies as a separate track. We want to be involved with these developments to ensure nwaku remains a useful client in very large networks and assist researchers in implementing scaling strategies in nwaku as reference implementation.

Overal roadmap is set by the Secure Messaging project in Vac Research.

### 7. Service incentivisation (collab with Vac Research)

Since we want to see Waku running everywhere, nwaku will benefit from any progress in adding incentivisation/disincentivisation mechanisms to protocols. As the reference implementation we want to be useful in helping the research effort reach production-readiness.

Overall roadmap is set from within Vac Research.

### 8. Peer management

Proper [peer and connection management](https://github.com/waku-org/pm/issues/6) is crucial to ensure reliable message delivery in a Waku network, ensure network integrity, enhance security, etc.

#### First milestones

- Milestone 1: [Networking MVP](https://github.com/waku-org/nwaku/issues/1353)

### 9. Hiring
 
We want to add at least three new hires within the year to solidify our team and expand our capabilities. We roughly need two general software developers to help with development, devops, and support work, and a third with specific low-level programming and optimization skills.

### 10. Network testing/Kurtosis simulations (collab with Wakurtosis)

The main aim of network testing is twofold:
- simulate and benchmark Waku performance as it scales
- test Waku network robustness against adverse and difficult network conditions

#### First milestones

- Milestone 1: [First network test](https://github.com/logos-co/wakurtosis/issues/7)

### 11. Integrated testing environment (collab with Logos Devops)

This could benefit from the network testing infrastructure created in collab with Wakurtosis. We have very few regular tests that cover the integrated functionality of a nwaku clients, interoperability with other clients and no tests that ensure that our compiled binaries function as expected. In short, we need proper ["black-box testing"](https://www.wikiwand.com/en/Black-box_testing)that run at regular intervals and tests interoperability with other Waku clients.

### 12. Improved processes

There are several internal processes that can be streamlined and automated to free the team up to focus on more pressing productionisation and support issues. Many of these are related to devops and are already summarised in our [DevOps wishlist for 2023](https://notes.status.im/nwaku-devops-work). 

#### First milestones

- Milestone 1: [Automate release process](https://github.com/waku-org/nwaku/issues/611)