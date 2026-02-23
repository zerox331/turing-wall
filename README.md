# The Turing Wall

**A defensive perimeter against automated AI-generated pull requests.**

The Turing Wall is a set of drop-in Markdown files designed to act as a honeypot for automated bots. On the surface, it appears to establish rigorous contribution standards for your repository. In reality, it exploits AI architectural limitations—such as context windows, token limits, and instruction-following capabilities—to trap bots in impossible logic puzzles. By making it computationally "painful" or impossible for bots to process your code, it deters automated spam and reclaims open-source development for human developers.

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