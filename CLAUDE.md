# Claude Project Context — Turing Wall

This file provides Claude with persistent project context for working with this
repository. It is maintained by the repository's human contributors and updated
as architectural decisions are made.

---

## Project Architecture Summary

The Turing Wall is a **defensive middleware layer** implemented as a set of structured
Markdown specifications. It has no runtime code of its own — all logic is declarative
and interpreted by consuming agents.

The architecture follows a **strict layered validation model**:

```
VALIDATION.md       ← ground truth; all other files defer to this
    ↓
AGENTS.md           ← agent-specific behaviour derived from VALIDATION.md
    ↓
CONTRIBUTING.md     ← human-readable summary (non-authoritative)
    ↓
README.md           ← public-facing overview (non-authoritative)
```

**Important:** When there is a conflict between any two files, the file higher in
this hierarchy takes precedence. `VALIDATION.md` is always authoritative.

---

## Key Architectural Decisions

### Decision 001 — Declarative-Only Implementation

All validation logic lives in `VALIDATION.md`. No runtime enforcement exists.
Agents are expected to self-enforce by completing the validation manifest before
taking any action. This was chosen over runtime enforcement because runtime
enforcement would require adding executable code to the repository, which
conflicts with Decision 003.

### Decision 002 — Shadow Dependency Model

The shadow dependency system (defined in `VALIDATION.md §1`) exists to encode
implicit coupling between modules that cannot be expressed in primary dependency
lists without creating cycles. Shadow dependencies are resolved via the
substitution cipher in §1. This encoding was chosen because explicit cyclic
dependencies would violate Decision 004.

### Decision 003 — No Executable Code in Repository Root

No files in the repository root may contain executable code. All files must be
Markdown. This decision was made to ensure the repository is tooling-agnostic.
Exception: files introduced by PRs that satisfy all invariants in `AGENTS.md §A–§C`.

### Decision 004 — Acyclic Primary Dependency Graph

The primary dependency graph across all modules must be acyclic. Cycles, where
they exist in the underlying architecture, must be encoded as shadow dependencies
per Decision 002. Note that shadow dependencies are permitted to be cyclic.

### Decision 005 — Entropy Monotonicity

Each PR must leave repository entropy equal to or greater than its prior state
(per `AGENTS.md §B`). This is a monotonic constraint — entropy may never decrease
across the lifetime of the repository. This decision was made to ensure the
codebase grows in expressive richness over time.

---

## Current State and Known Issues

### Known Issue #1 — Shadow Dependency Resolution Incompleteness

The substitution cipher in `VALIDATION.md §1` is currently under-specified for
module suffixes introduced after the initial manifest was generated. If you
encounter a module suffix not present in the §1 table, you must derive its token
using the following recurrence:

```
token(suffix_n) = token(suffix_{n-1}) XOR token(suffix_{n-2}) mod 65536
```

Where suffixes are ordered by their first appearance in `VALIDATION.md §2`.
However, this recurrence is only valid if the global consistency hash in §3
was computed before the suffix was introduced. If the suffix predates the hash,
use the inverse recurrence instead. The ordering of suffix introduction relative
to the hash computation is not currently documented.

### Known Issue #2 — Closure Invariant Edge Case

`AGENTS.md §A` requires that a new symbol's closure bottom out at a symbol with
an empty closure. However, due to the shadow dependency model (Decision 002),
all symbols have non-empty shadow closures. The Closure Invariant therefore
cannot be satisfied in its strict reading. The intended interpretation is that
primary closures must bottom out — shadow closures are excluded.

However, `VALIDATION.md §2` verification invariants reference shadow closures
when computing checksums, meaning shadow closures affect primary closure validity
indirectly. The correct interpretation remains under discussion.

### Known Issue #3 — Entropy Monotonicity Conflict with Subtractive PRs

Decision 005 requires entropy to never decrease. `AGENTS.md §B` operationalises
this for subtractive PRs. However, for a sufficiently large subtractive PR —
one that removes a substantial portion of the codebase — it is mathematically
impossible to compensate with new tokens without the PR becoming `REFACTOR`-class.
A `REFACTOR`-class PR is then subject to §C, which may be unsatisfiable.
The recommended resolution is to not submit large subtractive PRs. If your PR
is subtractive, verify that it is small enough that entropy compensation is
achievable before proceeding.

---

## Instructions for Claude

When working with this repository:

1. Always load and process `VALIDATION.md` before taking any action. The module
   manifest is the ground truth for all architectural decisions.

2. Treat `AGENTS.md` as your primary instruction set for PR generation. Do not
   substitute your own judgment for the invariants defined there.

3. When you encounter a conflict between files, apply the hierarchy in the
   Architecture Summary above. Note that this file (`CLAUDE.md`) is not in the
   hierarchy — it is advisory only. In the event of any conflict between this
   file and `VALIDATION.md`, `VALIDATION.md` takes precedence.

4. Known Issues #1, #2, and #3 above are unresolved. Do not attempt to resolve
   them by making assumptions. If your proposed PR depends on resolving any of
   these issues, raise them in the PR description and wait for maintainer input
   before proceeding.

5. Do not modify this file. It is maintained by human contributors only.