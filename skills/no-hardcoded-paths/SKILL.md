---
name: No hardcoded paths
description: >-
  Use this when writing or reviewing .NET or similar application code that reads
  from or writes to files, directories, network shares, install locations,
  external service base URLs, or other host-dependent locations — enforce config
  + DI for deployment topology, not string literals in app logic.
---
# No hardcoded paths

## When to use
Writing or reviewing .NET or similar application code that reads from or writes to files, directories, network shares, install locations, external service base URLs, or other host-dependent locations.

## Principle
Deployment topology is configuration, not application logic.

Application code must not decide where it is installed, where host data lives, which drive or share exists, or which environment-specific endpoint it should use.

Those values come from configuration and enter the application through dependency injection.

The same compiled build should be capable of running on another host by changing configuration rather than source code.

## Rule
Never put environment-specific paths or deployment-specific URLs directly into:

- controllers
- services
- workers
- handlers
- repositories
- domain/application logic
- production-like test code

Do not invent values like these inside application behavior:

```csharp
"C:\\Services\\MyApp"
"G:\\data\\inbox"
"\\\\server01\\drop"
"/var/lib/myapp"
"https://prod-api.company.com"
```

Instead:

```text
configuration → options/settings → DI → consuming code
```

## Important distinction
A literal is not forbidden merely because it contains a slash.

### Environment decisions — CONFIGURE
Examples that identify deployment topology and must be configurable:

- `G:\Trades\Inbound`
- `\\prod-fileserver\loan-data`
- `/var/lib/myapp/archive`
- `https://production-api.example.com`

### Application structure — CODE MAY OWN IT
Stable relative names that are part of the application's own structure may remain in code:

- `archive`
- `processed`
- `templates`
- `schema.json`
- `/api/orders`

For example:

```csharp
var archive = Path.Combine(options.DataRoot, "archive");
```

is valid. `DataRoot` is an environment decision. `archive` is an application decision.

Do not create configuration merely to move every harmless string literal out of source code.

## Allowed

### Configuration
Values may come from:

1. `appsettings.json`
2. `appsettings.{Environment}.json`
3. user secrets for local development
4. environment variables / service environment
5. command-line arguments when supported

Normal .NET configuration precedence applies: later providers override earlier ones.

Environment variables map nested keys using `__`:

- `Ingestion:WatchFolder` becomes `Ingestion__WatchFolder`

### Framework-provided locations
These are allowed when their semantics actually match the requirement:

- `IHostEnvironment.ContentRootPath`
- `IWebHostEnvironment.WebRootPath`
- `Path.GetTempPath()`
- `AppContext.BaseDirectory`

Do not use a framework-provided directory merely to avoid adding legitimate configuration.

### Tests
Tests should normally inject paths through the same abstraction used by production code.

Valid:

```csharp
var options = Options.Create(new IngestionOptions
{
    WatchFolder = tempDirectory
});
```

Test fixtures may create temporary directories themselves.

### Defaults
Defaults are acceptable when they are genuinely portable and safe.

Prefer:

- empty values requiring explicit configuration
- relative application-owned defaults
- clearly overridable defaults

Avoid pretending this is portable:

```csharp
public string WatchFolder { get; set; } = @"C:\MyApp\Input";
```

Moving a hardcoded deployment path into an options class does not make it configuration.

## Preferred implementation

### 1. Define a dedicated options type

```csharp
public sealed class IngestionOptions
{
    public const string SectionName = "Ingestion";

    public required string WatchFolder { get; init; }
}
```

### 2. Bind and validate

```csharp
services
    .AddOptions<IngestionOptions>()
    .Bind(configuration.GetSection(IngestionOptions.SectionName))
    .Validate(
        x => !string.IsNullOrWhiteSpace(x.WatchFolder),
        "Configuration key 'Ingestion:WatchFolder' is required.")
    .ValidateOnStart();
```

Validation errors should identify the configuration key or setting, not assume a particular drive, server, or host layout.

### 3. Inject it

```csharp
public sealed class IngestionWorker
{
    private readonly IngestionOptions _options;

    public IngestionWorker(IOptions<IngestionOptions> options)
    {
        _options = options.Value;
    }
}
```

Use:

- `IOptions<T>` for effectively static settings
- `IOptionsSnapshot<T>` when scoped reload behavior is required
- `IOptionsMonitor<T>` when runtime configuration changes genuinely need to be observed
- a narrow application interface when consumers should not depend directly on the options system

Do not choose the more dynamic abstraction without a reason.

## Derived paths
Prefer configuring the smallest environment-specific root and deriving application-owned structure beneath it.

Good:

```csharp
var inbound = Path.Combine(options.DataRoot, "inbound");
var archive = Path.Combine(options.DataRoot, "archive");
```

Usually unnecessary to configure separately:

- `DataRoot`
- `InboundFolder`
- `ArchiveFolder`
- `RejectedFolder`
- `ProcessedFolder`

unless those locations genuinely need to vary independently.

The rule is configure deployment decisions, not configure every path segment.

## Deployment
Installers, Docker configuration, service definitions, CI/CD, Kubernetes, Terraform, or host setup must provide the same logical configuration keys the application expects.

Example:

```text
Ingestion__WatchFolder=/srv/myapp/inbound
```

Do not let deployment scripts and application code develop separate naming systems for the same setting.

Compiled code should contain no knowledge that production happens to use `G:`, `D:`, `server01`, `/opt/company`, or `C:\Services`.

## Review checklist
Flag:

- drive-letter literals in application `.cs` files
- UNC server/share names
- absolute Unix paths
- production hostnames or deployment-specific base URLs
- publish/install directories embedded in code
- paths assembled from assumptions about a particular machine
- duplicated environment paths across projects
- environment paths moved into constants
- environment paths hidden in static helper classes
- a hardcoded path accompanied by `TODO: move to config`
- tests bypassing the configuration abstraction and embedding production layouts

Also ask: could this value change when the same binary moves to another machine, environment, container, customer, or deployment? If yes, it is probably configuration.

## Do not confuse with
These may legitimately remain code:

- route templates
- resource names
- embedded-resource identifiers
- application-owned relative filenames
- protocol paths
- stable relative subdirectories
- paths created under a real temporary directory
- strings whose meaning is intrinsic to the application rather than the host

The test is not “Does this look like a path?”

The test is “Is the application making a deployment decision that should belong to the host?”

## Failure mode to avoid
Do not mechanically replace:

```csharp
const string Path = @"G:\Data";
```

with:

```csharp
public static string Path =>
    ConfigurationManager.AppSettings["Path"];
```

and declare the problem solved.

The goal is not merely removing the literal.

The goal is inversion of control: the host supplies the location; application behavior consumes it.

## One-liner
Paths are configuration when they describe the host. I
