
# 2023 Secure Messaging Research (SeM) Priorities

> **Note:** These are *research* focus areas.
We cannot guarantee positive results, especially for highly explorative tasks.

## 1. Secure Scaling

The main research focus for SeM in 2023 is scaling [Waku Relay](https://rfc.vac.dev/spec/11/) to 1 million nodes.
This comprises several research challenges and tasks:

* efficient and privacy-preserving mapping of content topics to pubsub topics;
* discovery of pubsub topics;
* DoS mitigation.

We see DoS mitigation as the biggest challenge and potential blocker.
Depending on results, we might not be able to solve DoS issues satisfactory within 2023.
Our long-term approach to solving (spam-based) DoS is RLN,
which is mainly researched within [RLNp2p](https://rlnp2p.vac.dev/),
collaborating with the *SeM protocol incentivization* track.

Our [secure scaling roadmap](https://github.com/vacp2p/research/issues/154)
list issues leading towards our goals and will be kept up-to-date.

When scaling Waku Relay to 1 million nodes,
we expect other parts of Waku (besides discovery) to become bottlenecks;
especially the store, whose related research is part of the SeM *data synchronization* track.
We will identify new bottlenecks and think of ad-hoc fixes,
but will not focus research on these this year (not enough resources).

SeM will synchronize and collaborate with Waku product to keep research goals in line with practical needs.

SeM will also collaborate with [Kurtosis](https://roadmap.logos.co/roadmap/networking/status-waku-kurtosis/),
which will yield valuable test results, which can verify (and be verified by) our calculations.

## 2. Restriced Run

Aligned with our secure scaling goals, we will research allowing the restricted run protocols

* [Light Push](https://rfc.vac.dev/spec/11/)
* [Filter](https://rfc.vac.dev/spec/12/)
* [Peer Exchange](https://rfc.vac.dev/spec/34/)

to keep up with the above stated scaling goals.

## 3 Discovery

While the *discovery* track is not in focus as a driver this year,
discovery issues facilitating progress in secure scaling will be part of the focus.
This will comprise working on capability discovery,
as we plan to advertise pubsub topics as capabilities (or part of a hierarchical relay capability).

## 4. Privacy&Anonymity

We aim to progress on Waku Relay anonymity research.
We will continue working on [Waku Tor push](https://rfc.vac.dev/spec/47/), and tackle research problems coming up in the process.
SeM will integrate Tor push into nwaku, collaborating with the Nwaku team.

We will also work on a [Nym](https://nymtech.net/)-based solution (including an RFC), and a prototype.

## 5. Incentivization

We will work on a Service Credentials solution for service-offering protocols such as Store and Filter.
This will comprise both a specification and a prototype.

## 6. RLN

RLN is our long-term solution for protecting against spam-based DoS.
This is very important to achieve our scaling goals.
We will continue working on RLN.

More specifics: TBD.