## Suggestion: RFC Process Update

> *Note*: This is a basis for a Discussion on Discord.

Suggestions to update the RFC process defined in [1/COSS](https://rfc.vac.dev/spec/1/):

There are 2 main issues I'd like to address:

1) make publishing specs easy for other teams (without blockers), while Vac keeps quality control / advisory role
2) introduce a process for changing the responsible editor of an RFC

## RFC Process

Every team can publish raw RFCs, with reviewers from their respective team or other teams (could also be Vac).
Raw RFCs can be published without any formal process.
This allows all teams to publish specs without being blocked.

Promoting RFCs from raw to draft, as well as updating draft RFCs SHOULD involve Vac.
Promoting RFCs from draft to stable MUST involve Vac.
Vac would check various aspects of RFCs before promoting:

* spec parlance?
  - if not, editing PR (we plan to hire a spec editor)
* structure? (see template)
* if Standards Track: described well enough to facilitate independent interoperable implementations?
* ...

(future: where applicable: formal verification for a stable spec; would require more resources)

This is not a blocker, because teams can evolve and improve the spec in raw state.
Similar to the IETF RFC process, where draft specs are used in production long before they become an RFC.

Since raw RFCs can be added without an entry barrier, and some might be experimental,
I would suggest not allocating RFC numbers to RAW RFCs.
We could either completely remove numbers for raw RFCs (this is what the IETF does, only stable RFCs get a number),
or give them numbers in a different range, e.g. prefix a number with R: `R43/WAKU-TEST`.
The latter would allow referencing by number, but the number could change when promoting to draft.

## Version Numbers for draft RFCs

Suggestion: Introduce a version number for draft RFCs.
This is similar to the IETF process.
When applying conceptual changes (more than editing / restructuring) to a draft RFC,
the version number changes.
This version number is reflected in the file name, e.g. `README_01.md` as well as in the header of the respective draft RFC.
The header also contains links to old versions.
Older versions are *not* linked in the menu of `rfc.vac.dev`, and the name of the draft also does *not* show the version name.
(This is to avoid visual clutter.)

Advantages of this are:
* it is easy to reenact the change history
* the responsible editor can be changed with a version update, while the old version still shows the old editor

## Editor Updates

[1/COSS](https://rfc.vac.dev/spec/1/) states:

> A specification MUST have a single responsible editor, [...]

We do not have a process for changing the Editor, e.g. if an editor leaves or wants to move ownership.
Editor changes SHOULD only be done in consensus with the Editor.
For RAW RFCs, the editor can simply be changed.
For draft RFCs, the editor can be changed after a version update (see above).
For stable RFCs, the editor cannot be changed. Updates to a stable RFC that would require the RFC editor can only be done in a new RFC (with new number), updating the old RFC.






