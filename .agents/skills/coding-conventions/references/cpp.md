# C++ conventions

## Standard base

ISO/IEC 14882:2024 (C++23) is the default (`-std=c++23`; `-std=c++2b` on a
compiler that does not accept `c++23` yet). A project may pin an older standard
(`-std=c++20`, `-std=c++17`) and records the reason. The C++ Core
Guidelines are the rule source. C++ has no official naming style, so the
style below is used unless the project already has another (rule 1 of the
overall skill).

## Naming

| Element | Convention | Example |
| --- | --- | --- |
| Class, struct, enum, concept, type alias | `PascalCase` | `StayReader`, `Stay` |
| Function, method, variable, parameter | `snake_case` | `read_stays()`, `byte_count` |
| Data member (private) | `snake_case` with trailing underscore | `buffer_` |
| Struct public data member | `snake_case`, no underscore | `check_in` |
| Constant (`constexpr`, namespace-scope `const`) | `kPascalCase` | `kMaxRetries` |
| Enum class value | `PascalCase` | `Status::NotFound` |
| Namespace | short `snake_case`; no `using namespace` in headers | `billing` |
| Template parameter | `PascalCase` | `typename ItemT` |
| Macro | `UPPER_SNAKE` with project prefix; avoid macros | `BILLING_ASSERT` |
| Header / source file | `snake_case.h` / `snake_case.cpp`, same base name | `stay_reader.h` |
| Header guard | `#pragma once` (or `PROJECT_PATH_FILE_H`) | |

## Project layout

Layers follow the `clean-architecture` skill ("Folder structure" and "Names per
language"). In C++ each layer is a directory and a `snake_case` namespace:

```
myapp/
├── src/
│   ├── domain/            # namespace myapp::domain: entities/, value_objects/
│   ├── application/       # namespace myapp::application: use_cases/, ports/
│   ├── infrastructure/    # namespace myapp::infrastructure: persistence/
│   └── presentation/
│       └── features/orders/
│           ├── order_list_view.h, order_list_view.cpp
│           └── order_list_view_model.h, order_list_view_model.cpp
└── tests/                 # unit/, integration/, e2e/ (test_<module>.cpp)
```

## Formatting

- `clang-format` with one checked-in config; 4 spaces (or the project's
  setting); braces on every control-flow body.
- Include order: matching header, project headers, third party, standard
  library; each group sorted.
- One declaration per line; declare at first use, initialise with `{}`.

## Language rules

- **Ownership:** RAII everywhere. No owning raw pointers, no naked
  `new`/`delete`; use `std::unique_ptr` by default, `std::shared_ptr` only for
  real shared ownership, created with `std::make_unique`/`make_shared`.
- Follow the rule of zero; if you define one of destructor, copy or move,
  define or delete all five.
- Pass by `const&` (large, read-only) or by value (small or sink); use
  `std::span` and `std::string_view` for non-owning views, `std::optional` for
  "maybe", `std::variant` for alternatives.
- `const` and `constexpr` by default; mark single-argument constructors
  `explicit`; mark `override`/`final`; `[[nodiscard]]` on results that must
  be used.
- Prefer algorithms and range-for over hand-written loops; `enum class` over
  plain `enum`; `nullptr` over `NULL` or `0`; `using` over `typedef`.
- No C-style casts; use `static_cast` and friends. No mutable global state.
- Headers are self-contained and contain declarations, templates and
  `inline` definitions only.

## Errors

Use exceptions for exceptional failures, or `std::expected` and error codes
where the project forbids exceptions; one choice per project. Destructors
never throw. Catch by `const&`; never `catch (...)` without rethrowing or
logging.

## Tests

GoogleTest, Catch2 or doctest; run under sanitizers (`address`, `undefined`)
in at least one build.

## Tooling

`clang-format`, `clang-tidy` with the Core Guidelines checks, compiler
warnings (`-Wall -Wextra -Wpedantic`), sanitizers.

Review with `QC-CPP-001`.
