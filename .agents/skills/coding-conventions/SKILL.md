---
name: coding-conventions
description: Programming conventions for writing or reviewing source code — naming, layout, formatting and language idioms — for Python, C, C++, C#, Rust, Flutter (Dart), React (TypeScript) and Shell (bash), and for YAML files and CI workflows (Gitea Actions, GitHub Actions). Use when writing, editing, reviewing or refactoring code in one of those languages, choosing names, setting up a formatter/linter config, or adding conventions for another language. Holds the rules shared by every language and points to a per-language sub-skill.
---

# Coding Conventions

One skill for all languages. This file holds what is true in every language;
everything specific to one language is in a sub-skill that is read only when
that language is in hand:

| Language | Sub-skill | QC checklist |
| --- | --- | --- |
| Python | `references/python.md` | `framework/qc/code/qc-programming-python.md` (`QC-PY-001`) |
| C | `references/c.md` | `framework/qc/code/qc-programming-c.md` (`QC-CL-001`) |
| C++ | `references/cpp.md` | `framework/qc/code/qc-programming-cpp.md` (`QC-CPP-001`) |
| C# | `references/csharp.md` | `framework/qc/code/qc-programming-csharp.md` (`QC-CS-001`) |
| Rust | `references/rust.md` | `framework/qc/code/qc-programming-rust.md` (`QC-RS-001`) |
| Flutter (Dart) | `references/flutter.md` | `framework/qc/code/qc-programming-flutter.md` (`QC-FL-001`) |
| React (TypeScript) | `references/react.md` | `framework/qc/code/qc-programming-react.md` (`QC-RX-001`) |
| Shell (bash) | `references/shell.md` | `framework/qc/code/qc-programming-shell.md` (`QC-SH-001`) |
| YAML files, CI workflows | `references/yaml.md` | `framework/qc/code/qc-yaml.md` (`QC-YML-001`), `framework/qc/code/qc-workflow.md` (`QC-WF-001`) |

Read the sub-skill for the language you are working in, then write or review
the code. To review, use the language's QC checklist and record the result as
an `RC-*` (see the `artifact` skill).

## Precondition: a planned task

Before writing or editing code under `src/` or `tests/`, name the task row
(`MIL-NNN`, task N) or the synced issue, and the use case or design artifact
it implements, or say it is a plain technical task. If you cannot, refuse and
use the `project-planning` skill instead (rule: `framework/process/plan-first-gate.md`).
The task's milestone must be `Accepted` with a `Go` review record; if it is
still `Proposed`, or its latest review is conditional or No-Go, stop and say so.
Reviewing code needs no task. Before the pull request, code is reviewed against
the checklist of its language (`qc-programming-*`) and the result is recorded
as an `RC-*`.

## Rules for every language

1. **The existing code wins.** In a file or project that already has a
   convention, follow it, even if the sub-skill says otherwise. Do not mix
   styles within a file; do not reformat code you are not changing.
2. **Naming follows the language, not your habits.** Casing differs per
   language (table below). Never carry one language's casing into another.
3. **A name says what, not how.** Name by purpose in the domain's language
   (IT Professional English, as the registry's `Languages` section says), not by
   type or implementation (`customer_list`, not `arr2`).
4. **Length follows scope.** Short names (`i`, `n`) only for tiny scopes;
   wider scope, longer name. No abbreviations except ones the whole domain
   uses (`id`, `url`, `http`).
5. **Booleans read as a question:** `is_valid`, `has_items`, `can_retry`
   (cased per language). No negated names (`is_not_ready`).
6. **Functions are verbs, types are nouns.** A function that returns a value
   without side effects may be a noun (`total`, `Total`) where the language
   community does so.
7. **Formatting is done by the formatter,** not by hand and not in review
   comments. Each sub-skill names the formatter and linter; commit its
   configuration file with the code.
8. **Comments say why,** never what the code already says. Public APIs get
   the language's documentation-comment form.
9. **No dead or commented-out code, no unexplained magic numbers.** Name the
   constant.
10. **Errors are handled or propagated, never swallowed.** Each sub-skill says
    how its language does this.
11. **State each piece of knowledge once (DRY).** A business rule, constant,
    format, validation or query is defined in one place and used from there.
    DRY is about knowledge, not about text: two blocks that look alike but
    change for different reasons stay separate, because merging them couples
    what should change independently. Common mistakes: copying a block to save
    time; merging two look-alikes into one function full of flags.
12. **Dependencies point inward.** Business rules do not depend on the
    framework, the database, the user interface or the delivery mechanism;
    those depend on the rules. Code imports (includes, references) only what it
    uses, outer code supplies its implementations through an interface the
    inner code defines, and there are no cycles. Test: can the business rules
    run under test with no database, web server or framework? Common mistake:
    domain code that imports the ORM or the web framework for convenience. The
    `clean-architecture` skill gives the layers, ports and composition root.
13. **SOLID, where the language has classes or interfaces (Python, C++, C#).**
    In C and shell, keep each module or script to one responsibility. Each
    principle with its common mistake:
    - *Single responsibility*: a class or module has one reason to change. A
      class whose purpose needs "and" to describe has two.
    - *Open-closed*: add behaviour by adding a type, not by editing a type test
      (`if`, `match`, `switch` on a kind) in several places. The same switch
      repeated in several functions is the sign.
    - *Liskov substitution*: a subtype works wherever its base is expected and
      keeps the base's contract. A subclass that refuses an inherited operation
      breaks it.
    - *Interface segregation*: interfaces are narrow, one per consumer need. One
      large interface where each user needs two methods breaks it.
    - *Dependency inversion*: high-level code depends on an abstraction it owns
      and receives the implementation. Constructing the concrete database client
      inside the business rule breaks it.

    The QC checklist of the language has the matching criterion for rules 11 to
    13; it says what a reviewer checks and does not restate the rule.
14. **Every project starts with a `.gitignore` for its type.** Take the
    template for the language or toolchain from GitHub's gitignore templates,
    through the API and not from a web page, add it in the same change that
    creates the project, and append project-specific entries (local `.env`,
    generated output) below it. Do not write one from memory, and do not commit
    build output, caches or secrets to get a green build.

    Find the template name, then download it (the answer is JSON; the file is
    its `source` field):

    ```bash
    curl -L -H "Accept: application/vnd.github+json"       -H "X-GitHub-Api-Version: 2026-03-10"       https://api.github.com/gitignore/templates

    curl -L -H "Accept: application/vnd.github+json"       -H "X-GitHub-Api-Version: 2026-03-10"       https://api.github.com/gitignore/templates/<NAME> | jq -r .source > .gitignore
    ```

    `<NAME>` is a name from the list: `VisualStudio` for C#, `Python`, `C`,
    `C++`. Review the file before it is committed.
15. **Line endings are LF, always** — never CRLF or CR, on every platform and
    in every file (source, Markdown, YAML, scripts). Commit `.gitattributes`
    with `* text=auto eol=lf` and `.editorconfig` with `end_of_line = lf` (rule 18), and
    one newline at the end of a file. A tool that writes a file on Windows must
    be told to keep LF (for example `newline="\n"` in Python); check the diff
    for whole-file changes before it is committed.

16. **File and folder names are English and have no space.** Every file and
    folder name is written in English (IT Professional English), contains only
    letters, digits, `-`, `_` and `.` (no space, no accented or other national
    letter), and follows the casing of its language (table below; `kebab-case`
    for documents and shell scripts). The names of the layers and folders of
    the project follow the `clean-architecture` skill ("Folder structure" and
    "Names per language"). Exceptions are the names a tool or a convention
    fixes: `README.md`, `LICENSE`, `CHANGELOG.md`, `AGENTS.md`, `SKILL.md`,
    `Dockerfile`, `__init__.py`, dot files such as `.gitignore`, and a wiki
    page, which is `Page-Name.md` with a `-<language>` suffix (`Home-da.md`).
    Common mistakes: a folder `New folder` or `my notes`, a file named in the
    Product Owner's language, and a Python module in `PascalCase`.

17. **Tests cover the kinds of behaviour below.** For each unit of behaviour,
    write the tests that apply and say in the Test Plan (`TP`,
    `docs/test-plan.md`) which kinds do not apply and why. The test framework and naming are in the sub-skill.

    | Kind | What the test proves |
    | --- | --- |
    | Positive (happy path) | Valid input gives the expected result |
    | Negative | Invalid input, missing data or a forbidden action is rejected with the specified error |
    | Boundary value | Behaviour at, just below and just above each limit (0, 1, max, max + 1, empty, full) |
    | Edge case | Unusual but valid input: empty collection, one item, null or missing optional value, Unicode, very large input, leap day, time-zone change |
    | Equivalence partitioning | One representative per class of input that the code treats the same; no test repeats a class |
    | State transition | Each allowed transition reaches the right state, and each forbidden transition is refused |
    | Concurrency | Parallel callers keep the data correct, with no lost update or deadlock |
    | Race condition | A forced interleaving (or a repeat loop) of two operations on shared state gives one valid outcome |
    | Asynchronous behaviour | Completion, ordering, cancellation and a failed task are observed; never wait with a fixed sleep |
    | Timeout and retry | A slow call times out after the set time; a retry runs the set number of times with the set back-off, and stops after success or after the last attempt |
    | Error handling | Each failure (exception, error code, unavailable dependency) is handled or propagated as specified, leaves no half-done state and logs without leaking secrets |
    | Integration | Two or more real parts (the database, the file system, a queue, an HTTP client) work together across the boundary |
    | Regression | Every fixed defect gets a test that fails without the fix and is kept |
    | Exploratory | Time-boxed, unscripted testing to a charter; each defect found becomes a scripted test (a regression test) |

    - A unit test uses a fake clock and a fake or in-memory port; it does not
      sleep or call the network. Integration tests run against the real
      technology (`tests/integration`).
    - Concurrency, race and timeout tests are deterministic: control the
      scheduler, clock or latch, and repeat only to widen the interleaving.
    - Exploratory testing has a written charter and a session note; it is
      the only kind that is not automated.

18. **Every code project has a root `.editorconfig`.** It is made from the
    templates in the framework: copy `templates/editorconfig/baseline.editorconfig`
    to the root of the repository as `.editorconfig`, and for a project with C#
    append `templates/editorconfig/csharp.editorconfig`. The baseline sets
    `root = true`, utf-8, LF, 2 spaces, a final newline and trimmed whitespace, with
    the exceptions for Markdown, `.cmd` and `.bat` (CRLF, with `.gitattributes`),
    Makefiles (tabs), Python and C# (4 spaces). A project may add sections for its
    own file types but does not weaken the baseline. `check-editorconfig.sh`
    fails a repository that has source files and no root `.editorconfig`.

## Casing at a glance

| Element | Python | C | C++ | C# |
| --- | --- | --- | --- | --- |
| Type / class | `PascalCase` | `snake_case_t` | `PascalCase` | `PascalCase` |
| Function / method | `snake_case` | `snake_case` | `snake_case` | `PascalCase` |
| Variable / parameter | `snake_case` | `snake_case` | `snake_case` | `camelCase` |
| Constant | `UPPER_SNAKE` | `UPPER_SNAKE` | `kPascalCase` | `PascalCase` |
| Private member | `_leading` | file-scope `static` | `trailing_` | `_camelCase` |
| Namespace / module | `snake_case` module | `mod_` prefix | `snake_case` | `PascalCase` |
| File | `snake_case.py` | `snake_case.c/.h` | `snake_case.cpp/.h` | `PascalCase.cs` |

| Element | Rust | Flutter (Dart) | React (TypeScript) |
| --- | --- | --- | --- |
| Type / class | `PascalCase` | `PascalCase` | `PascalCase` |
| Function / method | `snake_case` | `lowerCamelCase` | `camelCase` |
| Variable / parameter | `snake_case` | `lowerCamelCase` | `camelCase` |
| Constant | `UPPER_SNAKE` | `lowerCamelCase` | `UPPER_SNAKE` |
| Private member | no `pub` | `_leading` | not exported |
| Namespace / module | `snake_case` | `snake_case` library | `kebab-case` folder |
| File | `snake_case.rs` | `snake_case.dart` | `PascalCase.tsx` (component), `kebab-case.ts` |

The table is a summary; the sub-skill is authoritative. Shell (bash) is not in
the table: functions and variables are `snake_case`, constants and environment
variables `UPPER_SNAKE`, files `kebab-case.sh`.

## Governance boundary

Conventions are **defined and governed** here but **not enforced** by this
framework: writing a linter or CI job that enforces them is outside the
framework's scope. The formatter and linter names in each sub-skill are the
recommended tools, not a pipeline. A new or changed convention follows the
process in the project's Coding Standards Governance document and is recorded
in `framework/CHANGELOG.md`. No project data belongs in this skill.

## Adding a language

1. Add `references/<language>.md` with these sections: Standard base, Naming,
   Formatting, Language rules, Errors, Tests, Tooling.
2. Add `framework/qc/code/qc-<language>.md` (`QC-<SHORT>-001`) using the `QC` type
   of the `artifact` skill, tagging every criterion with an ISO/IEC 25010:2023
   characteristic and a Level.
3. Add the row to the table above, the casing table, and a row for the
   language in `framework/registry/artifact-catalog.md`; note it in
   `framework/CHANGELOG.md`; run `bash framework/scripts/install-skills.sh`.
