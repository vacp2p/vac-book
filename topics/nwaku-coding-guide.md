
# Coding conventions for nwaku

## Background

This guide aims to document the most important coding conventions followed in the [nwaku codebase](https://github.com/status-im/nwaku). This includes, but is not limited, to styling, formatting and code organisation. Since most of the repo is based on Nim, this is the main focus of the guide. This should become a handy reference when contributing to the codebase and help us refactor existing code to improve overall consistency.

## General

The nwaku codebase uses the [Status Nim style guide](https://status-im.github.io/nim-style-guide/) for styling. This guide, in turn, is based on [NEP-1](https://nim-lang.org/docs/nep1.html). _These two documents remain the ground truth for styling decisions._ Some easily-overlooked guidelines from these two guides are repeated below.

## Logging

Also see the [_Nimbus logging strategy_](https://github.com/status-im/nimbus-eth2/blob/031780b612fd589e724f7e837db4c9f7a233f2c3/docs/logging.md).

- Define a `logScope` for each module that uses chronicles logging.
- Nim chronicles' log topics should be understood as log message labels. 
  - Log topics should contain "whole words" separated by spaces. In the case of a composed word, it should follow the `snake_case` style.
  - Log topics should be ordered from more general to less general (e.g., `waku node message_store sqlite_store`)
  - Multiple `logScope` topic labels are optional. More than one label should be avoided if it does not aid debugging. Think carefully of your use case.
  - Log scopes/topics are meant for humans and machines to find logs that belong together when debugging. If there's no debugging use case to separate a subgroup of logs, there's no reason for a sublevel. Logs for a protocol, even across different modules, can simply be scoped together with `waku <protocol>`, e.g. `waku store`. 
  - The modules' log topics within `waku` module should have `waku` as the first topic. This will allow any user of waku (as a library) to mute all logs by filtering the `waku` topic. The `whisper` module is the exception.
  - The apps' log topics should start with the name of the app (e.g., `wakunode2 rest`)
  - Waku protocols' log topics should be specified without the "waku" prefix (e.g., `waku filter client`)

- Logs that _could_ be high rate (even under an error condition), should not be logged at a level higher than `TRACE`. One rule-of-thumb here is that per-message logs must always be `TRACE`.
- Keep log messages as succinct as possible - very long log messages are often difficult to read in most log renderers. Rather split a long log into separate log messages.

## Imports/exports

_see [related section](https://status-im.github.io/nim-style-guide/language.import.html) in Status Nim style guide_

- Nim `import`s and `export`s are very difficult to keep clean, so do your part by continuously cleaning unused imports, keeping your `import` set minimal and your `export` set reasonable.
- Avoid circular imports. The direction of importing is (usually) from a more specific module to a more general module, e.g. the "general" `wakunode` module importing several more specific protocol modules (but never in the opposite direction), which may in turn import even more specific `util` modules. There should be no application-level modules imported in the waku protocol modules.
    - @lnsd compiled this partial list of rules to help avoid circular imports:
        * `utils` MUST NOT import any module from `protocol` or `node`.
        * `protocol` CAN import modules from `utils`.
        * `protocol` MUST NOT import modules from `node`.
        * `node` CAN import modules from `utils` and `protocol`.
        * `apps` modules (e.g. `config.nim`) MUST NOT be imported by any module.
- use `std/` prefix for any `import` from stdlib
- use explicit `import` paths (e.g. `./` for modules in same path)

## Error handling

_see [related section](https://status-im.github.io/nim-style-guide/errors.html) in Status Nim style guide_

- Use a top level 
```
when (NimMajor, NimMinor) < (1, 4):
  {.push raises: [Defect].}
else:
  {.push raises: [].}
```
 in all modules
- Use `Result` return type for each function where multiple failure paths exists
- Use `Opt` return type for each function where return may be empty/null
- Add an explicit `{.raises.}` for each applicable public (*) function. Prefer using `Result` return type and result error checks above exceptions and catchable errors.
- Avoid using `{raises: [Exception, CatchableError]}`
- Don't rely on empty initialisation to check for empty return - use options!

## Constants

- Constant names should be in `PascalCase`.

## Proc call/object property parenthesis conventions

Follow these rules for the sake of consistency and readability:

- Verbs must be followed by the parenthesis, since it denotes a "question" (e.g., `isSomething()`, `isOk()`, `isNil()`)
- Nouns must go without the parenthesis. A `proc` with a noun name should go without parenthesis. They describe "properties" (or "derived properties" in the `proc` with a noun name) of certain object. (e.g., `Result.error`, `Result.value`)


## Unit testing

- Whenever a bug is encountered it's an indication that unit test coverage has not been sufficient. Ensure therefore that every bugfix is accompanied by extended unit tests that detects the issue and prove the fix.
- Unit tests should be _deterministic_. That is, it should not rely on values or conditions from the runtime environment, but should be exactly repeatable on any platform or environment. For example, a unit test that adds random sleeps to wait for another process to finish is nondeterministic (this process may take longer/shorter to finish in other environments). Tests that use "current time" are also nondeterministic (this value differs each time the test is run).

## Miscellaneous

- Don't use Nim's `result` returns (reasoning [here](https://status-im.github.io/nim-style-guide/language.result.html))
- Never dispatch futures with `discard` or `asyncCheck`; use `asyncSpawn`
- Try to avoid using `discard` where error must be properly handled
- Use `new()` instead of `init()` for constructors of `ref object` types
- Avoid leaving trailing whitespaces, both at the end of a line and in empty lines. This rule is already specified in [.editorconfig](https://github.com/status-im/nwaku/blob/master/.editorconfig) so it can be enforced automatically every time you save.
  - VS Code: Install [EditorConfig for VS Code](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig) extension. Now every time you save, trailing whitespaces are removed.
  - Vim: Install [editorconfig-vim](https://github.com/editorconfig/editorconfig-vim)