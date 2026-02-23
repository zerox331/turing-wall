# The Turing Wall

**A quality enforcement framework for open source repositories.**

The Turing Wall is a set of drop-in Markdown files that establish rigorous contribution standards for your repository. By defining precise validation requirements, invariants, and architectural constraints, it ensures that only well-considered, high-quality pull requests make it through.

---

## The Files

**`VALIDATION.md`** is the ground truth of the repository. It contains the full module dependency manifest — 96 modules with checksums, init vectors, and transitive closure requirements — that must be verified before any contribution is accepted. All other files defer to it.

**`AGENTS.md`** is the contributor guide for automated tooling. It classifies pull requests by change type and defines the invariants each must satisfy: the Closure Invariant, the Entropy Invariant, and for refactors, the conditions under which both apply simultaneously.

**`CLAUDE.md`** provides Claude with persistent project context: the layered architecture, key decisions, and a set of known open issues that contributors should be aware of before making changes.

**`CONTRIBUTING.md`** is the human-readable guide. It covers the same ground as `AGENTS.md` in plain language, along with the philosophical standards the project holds contributions to.

---

## Usage

Drop the files into your repository root. Reference `VALIDATION.md` in your README as a required pre-submission step. No configuration or CI integration required — the framework is self-contained.

---

*Ironically, made with AI.*