# C# conventions

## Standard base

Microsoft's C# coding conventions and .NET Framework Design Guidelines, with
the analyzers that ship in the SDK. Target the language version of the
project's `LangVersion` / target framework.

## Naming

| Element | Convention | Example |
| --- | --- | --- |
| Namespace | `PascalCase`, matches folder path | `Billing.Stays` |
| Class, struct, record, enum, delegate | `PascalCase` (nouns) | `StayReader` |
| Interface | `I` + `PascalCase` | `IStayReader` |
| Method, property, event, public field | `PascalCase` | `ReadStays()`, `CheckIn` |
| Constant, `static readonly` | `PascalCase` | `MaxRetries` |
| Enum value | `PascalCase`; `[Flags]` enums are plural | `Status.NotFound` |
| Parameter, local variable | `camelCase` | `byteCount` |
| Private / internal field | `_camelCase` | `_buffer` |
| Generic type parameter | `T` or `T` + `PascalCase` | `T`, `TKey` |
| Async method | ends in `Async` | `ReadStaysAsync()` |
| Exception, attribute | end in `Exception` / `Attribute` | `InvalidDateException` |
| Boolean | `Is`, `Has`, `Can` prefix | `IsActive` |
| File | the type's name, one top-level type per file | `StayReader.cs` |

- Two-letter acronyms are upper case (`IO`); longer ones are `PascalCase`
  (`Xml`, `Http`).

## Project layout

Layers follow the `clean-architecture` skill ("Folder structure" and "Names per
language"). In C# each layer is a project named `<App>.<Layer>`, and the
namespace matches the folder path:

```
MyApp/
├── src/
│   ├── MyApp.Domain/          # Abstract/, Attributes/, Entities/, ValueObjects/, Repositories/, Errors/
│   ├── MyApp.Application/     # UseCases/, Dto/, Ports/
│   ├── MyApp.Infrastructure/  # Persistence/, Repositories/, Messaging/, Config/
│   └── MyApp.Presentation/
│       └── Features/Orders/
│           ├── OrderListView.xaml
│           └── OrderListViewModel.cs
└── tests/                     # unit/, integration/, e2e/
```

## Formatting

- `.editorconfig` checked in (the baseline plus `templates/editorconfig/csharp.editorconfig`, rule 18 of the skill); `dotnet format` applies it. 4 spaces, Allman
  braces, braces on every control-flow body.
- File-scoped namespaces (`namespace X;`); `using` directives outside the
  namespace, `System` first.
- No `var`: always write the explicit type (`IList<Stay> stays = ...`), also
  for `foreach` variables and `out` arguments; use target-typed `new()` to
  avoid repeating a long type. Enforce it in `.editorconfig` with
  `csharp_style_var_for_built_in_types`, `csharp_style_var_when_type_is_apparent`
  and `csharp_style_var_elsewhere` set to `false:warning`.

## Language rules

- Enable nullable reference types (`<Nullable>enable</Nullable>`) and treat
  nullable warnings as errors; do not suppress with `!` without a comment.
- `IDisposable` owners use `using`; implement the dispose pattern only when
  needed.
- `async`/`await` all the way; no `.Result` or `.Wait()`; no `async void`
  except event handlers; pass `CancellationToken` through public async APIs.
- Prefer properties over public fields, `readonly` and `init` for
  immutability, `record` for value-like data, pattern matching and switch
  expressions over long `if` chains.
- LINQ for queries, loops for side effects; do not enumerate a sequence twice.
- String interpolation over concatenation; `StringBuilder` in loops;
  `DateTimeOffset` over `DateTime` for points in time; `decimal` for money.
- XML documentation comments (`///`) on public types and members.

## Regions

Every type under `src/` groups its members in `#region` blocks, so the reader
gets a quick overview of the code: collapsed, a type shows its fields,
constructors, properties and methods at a glance. Regions are always used
there; the examples in this document omit them for brevity.

- Order and names, left out when the type has no member of that kind:
  `Fields`, `Constructors`, `Properties`, `Events`, `Public methods`,
  `Protected methods`, `Private methods`, `Nested types`.
- Each `#region` has a `#endregion` with the same name
  (`#endregion Fields`); regions are not nested.
- A region holds whole members and never splits one; do not wrap code inside a
  method body in a region.
- A type small enough to hold one kind of member (a record, an enum, an
  interface with only methods) still follows the rule when it has more than
  one kind.
- Tests under `tests/` are not bound by this rule.

## Observer pattern: `event` and delegates

Notify subscribers with the language's own Observer support: a delegate type
and the `event` keyword. Do not hand-roll an observer list
(`List<IObserver>` with `Attach`/`Detach`) and do not expose a public delegate
field.

```csharp
public sealed class StayReader
{
    // The publisher declares the event; subscribers only += and -=.
    public event EventHandler<StayReadEventArgs>? StayRead;

    private void OnStayRead(StayReadEventArgs e) => StayRead?.Invoke(this, e);
}

public sealed class StayReadEventArgs(Stay stay) : EventArgs
{
    public Stay Stay { get; } = stay;
}
```

- Declare events as `EventHandler<TEventArgs>` (or `EventHandler` without
  data); `TEventArgs` derives from `EventArgs` and is immutable. Use a custom
  delegate type or `Action<T>` only when the signature cannot fit
  `(object? sender, TEventArgs e)`, and say why in the documentation comment.
- Name the event for what happened, in the past tense (`StayRead`,
  `CheckedIn`); the raising method is `On` + the event name, `protected virtual`
  on an unsealed class, `private` on a sealed one.
- Raise through a null-conditional call, `Handler?.Invoke(this, e)`, so a
  subscriber removed on another thread cannot cause a `NullReferenceException`.
  The event is raised only from inside the publisher.
- A subscriber that lives shorter than its publisher must unsubscribe (`-=`),
  or the publisher keeps it alive. Unsubscribe in `Dispose`, or use a weak
  event pattern when the lifetime cannot be controlled.
- A handler must not throw into the publisher; catch what it can handle. When
  one failing handler must not stop the others, iterate
  `GetInvocationList()` and isolate each call.
- Handlers run synchronously on the raising thread. For long work, hand off
  inside the handler; for async work, use an async event handler (`async void`
  is allowed there only, see Language rules) or a channel/`IAsyncEnumerable<T>`
  instead of an event.
- In Clean Architecture the event type belongs to the layer that raises it. The
  Domain may raise events that Application and Infrastructure subscribe to;
  never the reverse.

## Hardware acceleration: SIMD and CUDA

Optimise for the CPU or the GPU only after a measurement shows the plain code
is too slow, and keep the plain code as the reference.

- **Keep a scalar implementation** of every accelerated routine. It is the
  fallback and the oracle: a test asserts that the accelerated result equals
  the scalar result (exactly for integers; within a stated tolerance for
  floating point, since reassociation changes rounding).
- **Prefer portable vectors first.** `System.Numerics.Vector<T>` or
  `Vector128/256/512<T>` adapt to the hardware and need no per-ISA code. Use the
  `System.Runtime.Intrinsics.X86` / `Arm` intrinsics (for example `Avx2`)
  only when a portable vector cannot express the operation.
- **Guard every intrinsic path** with `IsSupported` (`Avx2.IsSupported`,
  `Vector256.IsHardwareAccelerated`) and branch to the next-best path or the
  scalar one. Never assume the CPU of the build machine is the CPU of the
  target. Put the dispatch in one place per routine.
- **Handle the tail.** A vectorised loop processes `Vector<T>.Count` elements
  at a time; the remainder goes through the scalar loop. Prefer `Span<T>` and
  `ReadOnlySpan<T>` over arrays, and check bounds once before the loop.
- **Use CUDA through a library, behind a port.** Reach the GPU with a managed
  binding (ILGPU, ManagedCuda or a vendor library) in the Infrastructure layer,
  behind an interface the Application layer owns; the Domain never references
  CUDA. The CPU implementation of the same port is the fallback when no
  device is found. Copy data to the device in batches: a transfer costs more than
  a small kernel saves.
- **Document every improvement.** An accelerated routine carries, in its `///`
  comment or the ADR that introduced it:
  1. the hardware and runtime it was measured on (CPU model, GPU model, driver,
     .NET version);
  2. the input sizes and the benchmark (a BenchmarkDotNet project under
     `tests/`, checked in, so the figure can be reproduced);
  3. the scalar and the accelerated results, as a time and a speed-up ratio;
  4. the size below which the scalar path is faster, if the dispatch uses one.
  A change that cannot show a measured improvement is reverted, not kept.
- **Build with analyzers on.** Treat unsafe code and `Unsafe`/pointer use as
  a last resort, in a small, tested method, with a comment saying what the
  invariant is.

## Project setup

Add the `.gitignore` from the `VisualStudio` template when the solution is
created, downloaded through the GitHub gitignore API as rule 14 of `SKILL.md`
shows (`.../gitignore/templates/VisualStudio`).

Copy the three files in `framework/templates/csharp/` to the solution root
and change only what the comments say:

| File | Does |
| --- | --- |
| `Directory.Build.props` | Shared properties: nullable on, warnings as errors, `LangVersion`, code-style rules enforced in the build, one `g/` artifacts folder |
| `Directory.Build.targets` | Creates the intermediate and generated-code folders before a build |
| `.runsettings` | Test run: Cobertura coverage, migrations excluded |

A project does not repeat these properties in its own `.csproj`.

## Persistent data: the CRUD template

Persistent entities are not each given their own hand-written CRUD. Use one
generic base type, one generic contract and one generic implementation; the key
type is a type parameter, and the database column names are mapped separately.
Layers follow the `clean-architecture` skill.

| Layer | Holds |
| --- | --- |
| Domain | `IEntity<TKey>` (in `Abstract/`), `Entity<TKey>` and the entities |
| Application | `ICrudService<TEntity, TKey>` (the port) |
| Infrastructure | `CrudService<TEntity, TKey>` (EF Core) and one `IEntityTypeConfiguration<T>` per entity |

```csharp
// Domain/Abstract/IEntity.cs
public interface IEntity<TKey>
{
    TKey Id { get; set; }
}

// Domain/Entities/Entity.cs (IEntity.cs is not kept in Entities/)
public abstract class Entity<TKey> : IEntity<TKey>
{
    public TKey Id { get; set; } = default!;
}

// The default: a Guid key. A project's own entities derive from it:
//   public sealed class <YourEntity> : Entity { ... }
// A different key type is named explicitly:
//   public sealed class <YourEntity> : Entity<int> { ... }
public abstract class Entity : Entity<Guid>;

// Application
public interface ICrudService<TEntity, TKey>
    where TEntity : Entity<TKey>
{
    Task<TEntity?> GetByIdAsync(TKey id, CancellationToken ct = default);
    Task<IReadOnlyList<TEntity>> GetAllAsync(CancellationToken ct = default);
    Task<TEntity> CreateAsync(TEntity entity, CancellationToken ct = default);
    Task<bool> UpdateAsync(TEntity entity, CancellationToken ct = default);
    Task<bool> DeleteAsync(TKey id, CancellationToken ct = default);
}

// Infrastructure
public sealed class CrudService<TEntity, TKey> : ICrudService<TEntity, TKey>
    where TEntity : Entity<TKey>
{
    private readonly DbContext _db;

    public CrudService(DbContext db) => _db = db;

    public async Task<TEntity?> GetByIdAsync(
        TKey id, CancellationToken ct = default) =>
        await _db.Set<TEntity>().FindAsync([id], ct);

    public async Task<IReadOnlyList<TEntity>> GetAllAsync(
        CancellationToken ct = default) =>
        await _db.Set<TEntity>().ToListAsync(ct);

    public async Task<TEntity> CreateAsync(
        TEntity entity, CancellationToken ct = default)
    {
        _db.Set<TEntity>().Add(entity);
        await _db.SaveChangesAsync(ct);
        return entity;
    }

    public async Task<bool> UpdateAsync(
        TEntity entity, CancellationToken ct = default)
    {
        TEntity? existing = await _db.Set<TEntity>().FindAsync([entity.Id], ct);
        if (existing is null)
        {
            return false;
        }

        _db.Entry(existing).CurrentValues.SetValues(entity);
        await _db.SaveChangesAsync(ct);
        return true;
    }

    public async Task<bool> DeleteAsync(
        TKey id, CancellationToken ct = default)
    {
        TEntity? entity = await _db.Set<TEntity>().FindAsync([id], ct);
        if (entity is null)
        {
            return false;
        }

        _db.Set<TEntity>().Remove(entity);
        await _db.SaveChangesAsync(ct);
        return true;
    }
}

// Infrastructure: table, column name and key generation live here only.
// The key column is named <table name>_id unless there is a reason to differ.
public abstract class EntityConfiguration<TEntity, TKey>
    : IEntityTypeConfiguration<TEntity>
    where TEntity : Entity<TKey>
{
    protected abstract string TableName { get; }

    public void Configure(EntityTypeBuilder<TEntity> builder)
    {
        builder.ToTable(TableName);
        builder.HasKey(x => x.Id);
        builder.Property(x => x.Id)
            .HasColumnName($"{TableName}_id")
            .ValueGeneratedOnAdd();
        ConfigureEntity(builder);
    }

    // The entity's own columns, indexes and relations
    protected virtual void ConfigureEntity(EntityTypeBuilder<TEntity> builder)
    {
    }
}

// Per entity, only the table name (and its own columns) is written:
//   public sealed class <YourEntity>Configuration
//       : EntityConfiguration<<YourEntity>, Guid>
//   {
//       protected override string TableName => "<table_name>";
//   }
```

- The default key is a `Guid` (`Entity`). Choose `Entity<int>` or
  `Entity<long>` when the database should generate a compact, ordered key and
  the id is never exposed or merged across systems; say why in the entity's
  documentation comment. The CRUD code never names a concrete key type.
- `Domain/Abstract/` holds the base contracts (`IEntity.cs`); `IEntity.cs`
  is never placed in `Entities/`. `Domain/Attributes/` holds the domain's own
  attributes and references no framework.
- The key column is named `<table name>_id`; `EntityConfiguration` applies it.
  Deviate only to match an existing schema, by overriding it in the entity's
  configuration.
- The Domain and Application layers do not reference EF Core. The key column
  name and whether the database generates the value are
  set in the entity's configuration, never as attributes on the entity.
- The API receives and returns DTOs, never `Entity<TKey>` types. The CRUD
  service works with domain entities (or repositories behind a port), and the
  controller or endpoint maps between the two at the boundary. This keeps the
  API contract separate from both the database and the Domain model, so a
  column or entity change does not change the contract.
- Register the open generic once in the composition root
  (`services.AddScoped(typeof(ICrudService<,>), typeof(CrudService<,>))`).
- When an entity needs more than CRUD (a query by a business field, a rule on
  write), add a narrow port for that entity beside `ICrudService`; do not grow
  the generic contract.

## Errors

Throw specific exceptions (`ArgumentNullException`, custom types); validate
arguments at the public boundary (`ArgumentNullException.ThrowIfNull`). Catch
the narrowest type; `throw;` (not `throw ex;`) to rethrow. Never an empty
`catch`.

## Tests

xUnit v3 (`xunit.v3` package; not xUnit v2, NUnit or MSTest); names like `Method_Condition_Expected`; one behaviour
per test; no dependence on order, time or the network.

## Tooling

`dotnet format`, the .NET analyzers (`AnalysisLevel`, `EnforceCodeStyleInBuild`)
and optionally StyleCop.Analyzers, configured in `.editorconfig`.

Review with `QC-CS-001`.
