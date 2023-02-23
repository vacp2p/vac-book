*(Originally posted here https://notes.status.im/UAFvOj1tTQ6uW2li0ZeXfw?both in August 2022)*

# Zerokit positioning

In line with out general vision, we want to make privacy solutions that run anywhere. A big part of that is making ZK tooling and dapps easy to run in many different environments and contexts.

## Environments

We want to make it easy to use certain Zero Knowledge modules from other environments:

- Systems programming environments, including via a C API
- Standalone, command-line
- Mobile environments

This is useful on its own right and beyond just nwaku, though that is its first user and highest priority.

## Modules

In terms of specific modules:

- RLN
- Service credentials
- Other novel constructs that are relevant and mostly exist in the JS world right now

RLN is the first module and highest priority now.

## Upstream

In addition to this, there are certain upstream needs that facilitate the goal of having ZK run "everywhere". While strictly speaking outside of Zerokit, there's an opportunity to contribute here too:

- Making ZK easy to use from mobile (most targets now are PoCs/ad hoc browser)
- Simplifying pipeline from Circom to arkworks (go direct and skip going via WASM)
- Supporting things like Halo2 lib running in the browser

## Opportunities

Finally, there's prioritizing above based on specific needs that exists or we think are likely to exist:

- Making RLN cross-client is first target, via C API and nwaku/go-waku (and Circom for js-waku)
- RLN-lib/relay has been identified as something that we can push outside of Waku
- There's a need for making ZK app, including dealing with credentials, easy on mobile
- Currently Halo 2 isn't usable through browsers (needs WASM target)
- Big part of the ecosystem is moving from Hardhat to Foundry, and there's an opportunity to provide a ZK dapp building experience that doesn't require JS or JS tooling
    - This ties into Explorations and low hanging fruit that exists in applied ZK space

## Social aspects

In addition to above, working on this has several positive side effects:

- We control the code which makes it easier for us to do research and do new things (novel optimizations etc)
- We upskill both in terms of Rust and ZK
- These skills and tooling is useful for other ZK related efforts
- Contributing to ecosystem gives us goodwill and makes it easier to partner on specific problems
- It also makes more concrete that we as Vac are not just writing "Waku code"

## Sketch of priorities

Based on above, we can start to see specific targets.

1) Make Zerokit RLN module useful for nwaku RLN-relay (WIP, advanced)
    - critical path is merging it in nwaku
    - outside of this, there appears to be some MT optimizations that we want to do
3) Make Zerokit RLN module generally useful for people who want to build stuff with RLN ("from zero to hero")
    - basically zk-kit but not JS centric
    - probably requires some contract interaction
    - this may interact with our push to RLN-relay/lib as independent Nim/C library/effort
4) Start working on new module, specifically to serve Waku incentivization with fungible service credentials

As well as starting to hire a Rust engineer to be able to help out with some of the more tooling/upstream related efforts.
