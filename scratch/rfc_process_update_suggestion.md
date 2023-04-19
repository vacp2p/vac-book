## Suggestion: RFC Process Update

> *Note*: This is a basis for a Discussion on Discord.

Suggestions to update the RFC process defined in [1/COSS](https://rfc.vac.dev/spec/1/):

There are a few items I'd like to address:

1) make publishing specs easy for other teams (without blockers), while Vac keeps quality control / advisory role
2) version numbers for RFCs
3) introduce a process for changing the responsible editor of an RFC (without copying the RFC)

## RFC Process

Every team can publish raw RFCs, with reviewers from their respective team or other teams (could also be Vac).
Raw RFCs can be published without any formal process.
This allows all teams to publish specs without being blocked.

It is RECOMMENDED to involve Vac for promoting RFCs from raw to draft, as well as updating draft RFCs.
Promoting RFCs from draft to stable SHOULD involve Vac.
Involving Vac in the process will add the `vac` tag to the RFC.

Vac would check various aspects of RFCs before promoting:

* spec parlance?
  - if not, editing PR (we plan to hire a spec editor)
* structure? (see template)
* if Standards Track: described well enough to facilitate independent interoperable implementations?
* ...

This is not a blocker, because teams can evolve and improve the spec in raw state.
Similar to the IETF RFC process, where draft specs are used in production long before they become an RFC.

Where applicable, stable specs SHOULD be formally verified.
A formal verification is indicated by the `formally-verified` tag.

## Version Numbers of RFCs and Protocols

### Raw

Raw RFCs do not have version numbers.
Raw RFC specifying protocols reflect the version in the respective protocol identifier, e.g,
`/vac/waku/peer-exchange/2.0.0-alpha1`
Older versions of raw RFCs are not available on rfc.vac.dev (only in the git history).

Since raw RFCs can be added without an entry barrier, and some might be experimental,
I would suggest not allocating RFC numbers to raw RFCs.
We could either completely remove numbers for raw RFCs (this is what the IETF does, only stable RFCs get a number),
or give them numbers in a different range, e.g. prefix a number with R: `R86/WAKU-TEST`.
I would suggest the latter, which allows referencing by number.
The number can change when promoting to draft; e.g. `R86/WAKU-TEST` might be renamed to `74/WAKU-TEST` once promoting to draft state.

### Draft

Suggestion: Introduce a version number for draft RFCs.
This is similar to the IETF process.
When applying conceptual changes (more than editing / restructuring) to a draft RFC, the version number changes.
This version number is reflected in the file name, e.g. `README_01.md` as well as in the header of the respective draft RFC.
The header also contains links to old versions.
Older versions are *not* linked in the menu of `rfc.vac.dev`, and the name of the draft also does *not* show the version name.
(This is to avoid visual clutter.)

Advantages of this are:
* it is easy to reenact the change history
* the responsible editor can be changed with a version update, while the old version still shows the old editor

Draft RFCs specifying protocols MUST match their version number to the protocol ID version:

`/vac/waku/filter/2.0.0-beta1`

would be specified in version `1` of this draft RFC.

`/vac/waku/filter/2.0.0-beta13`

would be specified in version `13` of this draft.

(Protocol versioning is specified in [Waku2](https://rfc.vac.dev/spec/10/))

### Stable

Once an RFC becomes stable, it cannot be changed anymore.

>  Changes to stable specifications should be restricted to cosmetic ones, errata and clarifications.

(I suggest changing this should to a MUST, like the IETF RFC process does.)

A stable RFC specifying something for which no previous stable RFC exists holds the implicit version number `1`.
If a new version of the given protocol or artifact should be specified,
a new *raw* RFC has to be started, with a new RFC number.
The name of the new RFC SHOULD explicitly append `-V2` to the spec name.
E.g. when specifying changes to the stable protocol `74/WAKU-TEST`, a new raw RFC named `R104/WAKU-TEST-V2` would be started.
This new raw RFC could eventually evolve into `85/WAKU-TEST-V2`.

Stable RFCs specifying protocols use a protocol ID of the form:

`/vac/waku/relay/2.0.0`

Here `0.0` represents `V1` of the RFC.

If desired, we could use `1.0` for the first version, or even remove the less significant portion (`1`) to match the RFC version.
`V2` of the RFC could either use protocol version `1.0` or `0.1`, depending how big the changes are.


### Further Points

The name extensions `V2`, `V3`, ... SHOULD only be used for stable RFCs.
RAW RFCs MAY use these extensions (because adding them does not have to adhere to a formal process).

Two (more) non-stable versions of a protocol/artifact should only be maintained if:

* both versions have their respective use
* two competing proposals

In both cases, they SHOULD not be named `-V1` and `-V2`, because versioning indicates one will eventually replace the other.
In case of competing proposals, only one of them should move to `stable` (preferably only one should move to `draft`).

## Editor Updates

[1/COSS](https://rfc.vac.dev/spec/1/) states:

> A specification MUST have a single responsible editor, [...]

An editor change requires copying the RFC in a new raw RFC (with new number).

Suggestion: add the following "in-place" editor change process:

In-place editor changes SHOULD only be done in consensus with the Editor.
For raw RFCs, the editor can simply be changed.
For draft RFCs, the editor can be changed after a version update (see above).

For stable RFCs, the editor cannot be changed. Updates to a stable RFC that would require the RFC editor can only be done in a new RFC (with new number), updating the old RFC.
