# Agent Operating Rules

## Purpose

This repository is the durable handoff layer for Walker. VS Code/Copilot chat
history is not relied on as durable memory across devices, sessions, or
setups.

## Source of truth

Chats are helpful context only.

The durable source of truth is:

1. repository files
2. Git commits
3. pull requests
4. issues
5. tests

## Checkpoint status in this clone

- `d2852d1` - UNVERIFIED / unavailable in this clone
- Git verification result for `d2852d1` in the active clone: object absent
- `d2852d1` must not be treated as a baseline, verified checkpoint, ancestor,
  source of truth, or comparison point unless later recovered and verified in
  the active repository
- `4e997f0` - verified commit in this clone
- `4e997f0` is reachable from current HEAD
- `4e997f0` is not automatically an approved baseline; inspect actual changes
  before treating it as approved

## Verified repository state

- branch at verification: `copilot/vscode-last-conversation`
- HEAD at verification: `f04a3ca6ae1cf91bc18dd3a21256caaf1345d3bd`
- working-tree status at verification: clean

## Product rules

- Walker is an accessibility-first companion.
- Walker must understand Saudi Sign Language, resolve Arabic meaning, and
  respond usefully.
- Walker is demonstrated only when:
  - real Saudi Sign Language input
  - accepted model prediction
  - verified Arabic meaning
  - useful Walker response
- The following are not product proof:
  - camera access
  - hand detection
  - landmarks
  - generic contour gestures
  - mouse-drawn gestures
  - generic ASL substitutions
  - a static or animated avatar
  - status panels
  - canned responses
  - synthetic/unit tests alone
- The repository must stay local-first and use free, open-source resources.

## Isharah source rule

- Isharah is the only designated primary Saudi Sign Language reference and
  intended dataset source.
- Do not substitute Gallaudet, WLASL, MS-ASL, generic ASL datasets, generic
  gestures, mouse gestures, or guessed label mappings for Isharah.

Reference:

- Isharah
- https://snalyami.github.io/Isharah_CSLR/

Until applicable terms/permission are verified, none of the following is
authorized:

- Isharah acquisition
- training on Isharah data
- storage of Isharah-derived data
- redistribution of Isharah-derived data
- derived-data use
- trained-model use
- publication
- commercialization

This restriction does not block data-independent architecture, validation,
documentation, and synthetic contract testing.

## Phase status

- Phase 0.5 was completed as architecture/testing preparation only.
- Phase 0.5 is not a trained model.
- Phase 0.5 is not a working Saudi Sign Language product.

## Operating rules for future agents

Before any code or documentation edit, every future agent must:

1. read `README.md`, `docs/agent-operating-rules.md`, and
   `docs/current-handoff.md`
2. verify repository root, branch, HEAD, and working-tree status
3. state:
   - current phase
   - goal
   - exact files allowed to change
   - exact files forbidden to change
   - evidence it will produce
   - commands/tests it will run
   - known blockers
4. wait for explicit approval whenever the task asks for a plan-first gate
5. use commits/PRs/tests to reconstruct context when chat history is missing
6. keep product proof separate from prototype/demo progress
7. record durable changes in repository files rather than relying on chat
8. report actual commands, raw outputs, changed files, test results, evidence,
   blockers, and one honest final status: PASS, FAIL, or BLOCKED

Agents are explicitly forbidden from:

- claiming complete, working, passed, connected, or Saudi Sign Language-ready
  without real acceptance evidence
- claiming Walker understands Saudi Sign Language without the defined
  acceptance evidence
- treating hand or landmark detection as language understanding
- modifying protected legacy prototypes without explicit user approval
- inventing or guessing source datasets, labels, translations, checkpoint
  ancestry, or model results

## Documentation split

Keep these categories separate:

- project rules and constraints
- agent workflow rules
- current handoff state and next steps
