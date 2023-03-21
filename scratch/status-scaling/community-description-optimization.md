# Optimizing the `CommunityDescription` dissemination

## Context

This document describes a solution for using Sparse Merkle Trees (SMT) on IPFS to distribute members' public keys in an organization. The solution allows organizations/communities to efficiently manage and verify the membership of their members in a trustless manner.

This is done to prevent network overhead when broadcasting the `CommunityDescription` message, which will increase with the number of members in an organization/community

## Method

The proposed solution is to use Sparse Merkle Trees (SMT) on IPFS to distribute members' public keys. 

The SMT is constructed with a set of leaf nodes, where each leaf node represents a public key of a member. The SMT can be updated by adding or removing leaf nodes as members are added or removed from the organization. The SMT is then recalculated to generate a new root hash, which is used to identify the SMT on the IPFS network.

The SMT can be stored on IPFS by adding the root hash to the IPFS network. The root hash can then be shared with the members of the organization, so they can retrieve the SMT from IPFS.

When a member wants to verify the membership of another member, they can use the SMT's proof mechanism to verify the presence of the member's public key in the SMT.

Therefore, the `CommunityDescription` protobuf changes from -
```protobuf=
message CommunityDescription {
  uint64 clock = 1;
  repeated bytes members = 2;
  OrganisationPermissions permissions = 3;
  ChatMessageIdentity identity = 5;
  repeated OrganisationChat chats = 6;
  // ... other fields
}
```
to -
```diff=
message CommunityDescription {
  uint64 clock = 1; 
- repeated bytes members = 2;
+ bytes members = 2; // Note: we should be able to change repeated bytes to bytes as the wire type of both is the same (Type 2) (I may be wrong here)
  OrganisationPermissions permissions = 3;
  ChatMessageIdentity identity = 5;
  repeated OrganisationChat chats = 6;
}
```

> Note: I have yet to explore viability of this solution for the other `repeated` field which may hold large amounts of data (chats)


## Napkin Math

- 100 members
    - 100 leaf nodes
    - 7 levels.
    - Max 128 nodes
    - Storage required: 100 * 32 = 3,200 bytes
- 1000 members
    - 1000 leaf nodes
    - 10 levels 
    - Max 1024 nodes
    - Storage required: 1000 * 32 = 32,000 bytes = 32 kb
- 10,000 members
    - 10000 leaf nodes
    - 14 levels
    - Max 16384 nodes
    - Storage required: 10000 * 32 = 320,000 bytes = 320 kb
- 100,000 members
    - 100,000 leaf nodes
    - 17 levels
    - Max 131072 nodes
    - Storage required: 100000 * 32 = 3,200,000 bytes = 3.2 mb

The storage required is relatively less, and membership can be verified easily by the nodes. 

The size of the `CommunityDescription` remains constant with the number of members in the community.

> I have not verified the integrity of this math, please help!

## Security considerations

- Anyone can update the tree, but the owner distributes the `CommunityDescription`, hence, the owner becomes a single point of failure. If the owner node is compromised, an arbitrary CID can be distributed, leading community members to believe that there are a different set of members
    - This can be solved by few members keeping the member tree in memory, and by computing the root hash themselves. If the local CID matches the CID distributed by the owner, then the members can verify that the computation was done correctly. 

## Future Work

- The storage can be done on a variety of platforms which have support for content addressable storage (Codex?)
