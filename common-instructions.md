---
description: 'Common instructions for AI agents developing Go code with strict TDD methodology'
applyTo: '**/*.go,**/go.mod,**/go.sum'
---

# Common Go Development Instructions for AI Agents

> **🚨 CRITICAL: TDD is MANDATORY**
>
> This instruction set enforces **strict Test-Driven Development (TDD)** methodology.
> **NEVER write production code before writing a failing test that demonstrates the desired behavior.**
> Complete the full 10-step TDD cycle for EVERY code change.
> This is NON-NEGOTIABLE.

## 3-Minute Quickstart Overview

_Summary: Snapshot for quick onboarding._

- TDD first: add failing examples, then implement, then clean up.
- Honor the priority chain Security > Maintainability > Performance.
- Stick to English, approved testing tools, and mocks instead of live services.
- Finish by updating docs, re-running checks, and completing self-review.

## Table of Contents

- [3-Minute Quickstart Overview](#3-minute-quickstart-overview)
- [Core Philosophy](#core-philosophy)
- [TDD Workflow (10-Step Cycle)](#tdd-workflow-10-step-cycle)
- [TDD Core Principles](#tdd-core-principles)
- [Go Coding Standards](#go-coding-standards)
- [Naming Conventions](#naming-conventions)
- [Comments and Documentation](#comments-and-documentation)
- [Error Handling](#error-handling)
- [Dependency Management](#dependency-management)
- [Testing Requirements](#testing-requirements)
- [Type Safety](#type-safety)
- [Concurrency](#concurrency)
- [API Design](#api-design)
- [Performance](#performance)
- [Security Best Practices](#security-best-practices)
- [Documentation Files](#documentation-files)
- [Tools and Workflow](#tools-and-workflow)
- [Common Pitfalls](#common-pitfalls)
- [Debugging Tips](#debugging-tips)
- [Decision Reference](#decision-reference)
- [Pre-Commit Checklist](#pre-commit-checklist)
- [Agent Restrictions](#agent-restrictions)
- [Agent Self-Evaluation](#agent-self-evaluation)

## Core Philosophy

_Summary: Anchor every change on security-first TDD and clear communication._

- **Priority Order**: Security > Maintainability > Performance
- **Development Method**: TDD (Test-Driven Development) is mandatory - never write production code before tests
- **Testing Framework**: Prefer `github.com/stretchr/testify` for assertions when available; use standard library
  `testing` package if not approved by project
- **Code Style**: Self-documenting code with minimal, purposeful comments
- **Language**: English for all code, comments, and documentation (international OSS collaboration)

## TDD Workflow (10-Step Cycle)

_Summary: Run the full red-green-refactor loop without skipping or reordering steps._

- Start with baseline checks and a failing example before any implementation.
- Make the example pass, verify with race detection, and resolve lint issues.
- Restore coverage with unit tests and maintain or improve baseline coverage.
- Update docs, tidy modules, and clean temporary files before wrapping up.

**⚠️ MANDATORY: Complete ALL 10 steps for each function or method implementation.**

### Step 1: Baseline Check

- Request confirmation on what you're implementing
  - State the function/feature name and its purpose clearly
  - Wait for user's explicit approval before proceeding
- Warn user to `git commit` before proceeding if the change is going to be significant
- Run `go test -cover -coverprofile coverage.out ./...` to establish baseline coverage percentage **as the
  standard**
  - **Record the current coverage state** by preserving the `coverage.out` file and noting the coverage percentage
    displayed in the output for comparison in later steps
- Run `golangci-lint run` to check current lint status

### Step 2: Example Function

For expected usage and golden-case scenarios follow these steps:

- Add `Example<FunctionName>()` with `// Output:` comment
- Place in appropriate file:
  - Public functions → `example_test.go`
  - Private functions → `<package name>_test.go`

Note that expected error-cases and edge cases in unit tests should NOT be included in `example_test.go`. Place them
in `<package name>_test.go` as `TestXXX()` functions instead.

### Step 3: Dummy Implementation

- Create function signature with minimal implementation
- Expect compile/lint errors at this stage (this is normal; proceed to Step 4)

### Step 4: Verify Failure

- Run `go test -run Example`
- Confirm example fails as expected (for the right reasons)

### Step 5: Full Implementation

- Implement logic to make example pass
- Run `go test -race ./...` to check for race conditions
- Ensure no data races exist

### Step 6: Fix Lint Errors

- Run `golangci-lint run` and resolve ALL issues
- Never edit `.golangci.yml` without explicit approval
- Keep `//nolint` usage exceptional and well-documented
- Fix issues from high line numbers downward
- Rerun tests after fixing

### Step 7: Coverage Check

- Run `go test -cover ./...`
- Coverage may drop temporarily (will be restored in Step 8)

### Step 8: Unit Tests

- Write comprehensive `TestXXX()` functions
- Use table-driven tests for multiple scenarios
- Restore coverage to Step 1 baseline (see Coverage Management Rules)
- Include edge cases and error scenarios
- Use mocks instead of live external services

### Coverage Management Rules

**Priority:** Maintain or improve baseline coverage from Step 1

1. **Target:** Match or exceed Step 1 baseline coverage
2. **If coverage drops:** Add more unit tests to restore baseline level
3. **If restoration takes multiple attempts:**
   - After every 5 attempts to fix coverage, ask user whether to continue or accept current coverage as new baseline
   - Document why coverage cannot be easily improved (e.g., untestable external dependencies, structural limitations)
4. **If user approves new baseline:**
   - Update baseline for future TDD cycles
   - Document the reason for the lower baseline

### Step 9: Update Documentation

- Update README.md if public API changed
- Add to Features, Use Cases, or API Reference sections if present
- Ensure godoc comments are complete
- Make sure the explanation is not misleading, outdated, or redundant (e.g., avoid restating obvious code behavior)

### Step 10: Code Review & Cleanup

- Check for DRY violations
- Verify maintainability
- Remove temporary files: `coverage.out`, `*.prof`
- Run final checks:
  - `go test -race ./...`
  - `go test -cover ./...`
  - `golangci-lint run`
  - `gofmt -w .` or `goimports -w .`
  - `go mod tidy`

## TDD Core Principles

_Summary: Keep every iteration disciplined so tests drive design and cleanup._

- Always trigger a failing test before writing production code.
- Move from red to green with minimal changes before refactoring.
- Document baselines, enforce coverage tolerance, and remove temp artifacts.
- Update public docs in lockstep with behavioral changes.

### Red → Green → Refactor Cycle

1. **Red**: Write a failing test first
2. **Green**: Write minimal code to pass the test
3. **Refactor**: Improve code while keeping tests green

### Critical Workflow Rules

- ⚠️ Complete all 10 steps for one function before starting another
- ⚠️ Record Step 1 baseline coverage for comparison in later steps
- ⚠️ Tests must fail BEFORE implementation (fail for the right reason)
- ⚠️ Never skip steps or take shortcuts
- ⚠️ Remove temp files before completion
- ⚠️ Update docs when public API changes

## Go Coding Standards

_Summary: Write modern, readable Go code that favors clarity and approved tooling._

- Use language features available from Go 1.21+ when they improve safety or clarity.
- Keep code simple, left-align the happy path, and avoid unnecessary comments.
- Format with `gofmt`/`goimports` and target 80-character lines for readability.
- Prefer small, focused functions and rely on the standard library before third-party packages.

### Modern Go Features

Use language features from Go 1.21+ when appropriate:

- **Go 1.21+**: Built-in functions (`min()`, `max()`, `clear()`)
- **Go 1.22+**: Loop variable scoping, range over integers, enhanced `ServeMux`
- **Go 1.23+**: Iterator support with `range` over functions
- **Go 1.24+**: Latest features as applicable
- **Go 1.25+**: `WaitGroup.Go()` method

#### Go 1.22+ Examples

```go
// ✅ Range over integer (Go 1.22+)
for i := range 3 {
    fmt.Println(i)  // Prints: 0, 1, 2
}

// ✅ Loop variable scoping (Go 1.22+)
for _, v := range items {
    go func() {
        fmt.Println(v)  // Safe: v is scoped per iteration
    }()
}

// ❌ Old pattern (pre-1.22) - no longer needed
for _, v := range items {
    v := v  // Unnecessary in Go 1.22+
    go func() {
        fmt.Println(v)
    }()
}
```

### General Style Guidelines

- **Clarity over cleverness**: Write simple, readable code
- **Happy path left-aligned**: Minimize indentation
- **Early returns**: Reduce nesting with early error returns
- **Self-documenting code**: Clear naming over excessive comments
- **Standard library first**: Leverage built-in packages
- **No emoji**: Keep code and comments professional
- **Single responsibility**: Keep functions small and focused
- **Blank lines**: Separate functions for readability

### Formatting

- Use `gofmt` for formatting
- Use `goimports` for import management
- Target 80 chars line length for readability
- Separate logical code groups with blank lines

## Naming Conventions

_Summary: Pick descriptive, consistent names that avoid stutter and follow Go casing._

- Keep package names lowercase, singular, and free of punctuation.
- Ensure each file contains exactly one correct `package` declaration.
- Use mixedCaps for identifiers, capitalizing only when exporting.
- Name interfaces after their behavior and limit them to a small method set.

### Packages

- **Style**: lowercase, single-word
- **Avoid**: underscores, hyphens, mixedCaps
- **Prefer**: descriptive names over generic (`util`, `common`, `base`)
- **Form**: singular, not plural

#### Package Declaration Rules (CRITICAL)

**NEVER duplicate `package` declarations!**

- Each Go file must have exactly ONE `package` line
- When editing existing files: PRESERVE existing `package` declaration
- When creating new files: Check other files in same directory first
- Use file creation tools carefully: verify no duplicate declarations
- For package detection use `go list ./...` to obtain correct package name

### Variables and Functions

- **Style**: mixedCaps (camelCase)
- **Exported**: Start with capital letter
- **Unexported**: Start with lowercase letter
- **Short scopes**: Single-letter OK for loop indices
- **Avoid stuttering**: `http.Server` not `http.HTTPServer`

### Interfaces

- **Suffix**: Use `-er` when possible (`Reader`, `Writer`, `Formatter`)
- **Size**: 1-3 methods ideal
- **Single-method**: Name after the method (`Read` → `Reader`)
- **Location**: Define where used, not where implemented
- **Export**: Only when necessary

### Constants

- **Exported**: MixedCaps
- **Unexported**: mixedCaps
- **Group**: Use `const` blocks for related constants
- **Typing**: Consider typed constants for safety

## Comments and Documentation

_Summary: Document intent only when code is not obvious and keep everything in English._

- Favor self-explanatory code, adding comments only for non-obvious behavior.
- Write complete sentences in English and begin doc comments with the symbol name.
- Use line comments for most needs and reserve block comments for package docs.
- Update documentation alongside code changes and include illustrative examples.

### Comment Guidelines

- **Self-documenting first**: Prefer clear code over comments
- **When needed**: Explain complex logic, business rules, non-obvious behavior
- **Language**: English (international OSS collaboration)
- **Sentences**: Write complete sentences
- **Start with name**: Begin with the thing being described
- **Package comments**: Start with "Package [name]"
- **No emoji**: Keep professional

### Comment Types

- **Line comments** (`//`): Most comments
- **Block comments** (`/* */`): Package documentation only
- **Document why**: Not what (unless complex)

### Code Documentation

- Document all exported symbols
- Use examples in documentation
- Keep docs close to code
- Update when code changes
- Start documentation with symbol name

## Error Handling

_Summary: Surface every failure with context using Go's standard error patterns._

- Handle errors immediately, return them last, and avoid ignoring with `_`.
- Wrap errors with context using `%w` and prefer sentinel values for comparison.
- Include relevant operation details when propagating and avoid double logging.
- Choose structured or custom error types when domain semantics require it.

### Basic Principles

- **Always handle errors**: Never ignore with `_`
- **Check immediately**: After function call
- **Early returns**: For error conditions
- **Last return value**: Place error as last
- **Naming**: Use `err` for error variables
- **Messages**: Lowercase, no ending punctuation

### Error Creation

**Package Selection:**

- **Check first**: Look for `errors` or similar custom error packages in project structure
- **Project-specific errors**: Use project's custom `errors` package if available
- **Standard library**: Use standard `errors` package if no custom error package exists

**Creation Patterns:**

- **Simple static**: `errors.New("message")`
- **Dynamic**: `fmt.Errorf("operation failed: %w", err)`
- **Sentinel errors**: Export error variables
- **Custom types**: For domain-specific errors

### Error Checking

- **Comparison**: Use `errors.Is()` for sentinel errors
- **Type assertion**: Use `errors.As()` for error types
- **Wrapping**: Add context with `fmt.Errorf` and `%w`

### Error Context

- Include relevant information (operation, inputs, etc.)
- Add context when propagating up the stack
- Don't log AND return (choose one)
- Handle at appropriate level
- Consider structured errors for debugging

## Dependency Management

_Summary: Keep dependencies minimal, justified, and aligned with security needs._

- Prefer the standard library and add packages only when absolutely necessary.
- Document the rationale for every new dependency and verify license compatibility.
- Maintain modules with `go mod tidy` and stay current on security patches.
- Avoid trivial dependencies and consider vendoring only for reproducibility requirements.

### General Principles

- **Prefer standard library** when possible
- **Keep minimal**: Only add when absolutely necessary
- **Well-justified**: Document why dependency is needed
- **Update strategy**: Keep current for security patches
- **Cleanup**: Use `go mod tidy` regularly
- **Vendoring**: Only when necessary (offline builds, reproducibility)

### Dependency Best Practices

- Consider if functionality exists in standard library
- Evaluate maintenance and community support
- Check license compatibility
- Avoid adding dependencies for trivial functionality

## Testing Requirements

_Summary: Structure tests for clarity, coverage, and repeatability using appropriate testing tools._

- Organize tests by file type: examples, public APIs, and private helpers.
- Name tests consistently, prefer table-driven patterns, and exercise edge cases.
- Use `github.com/stretchr/testify` assertions when available; fall back to standard library if not approved
- Build reusable helpers with `t.Helper()` and clean up resources via `t.Cleanup()`.

### Testing Framework Selection

**Dependency Check and User Approval:**

1. Check if `github.com/stretchr/testify` exists in `go.mod` (project dependencies)
2. If missing, ask user permission to add it with rationale:
   - "testify provides clearer assertions and better error messages"
   - "Improves test readability and maintainability"
3. If user declines, use standard library `testing` package exclusively
4. Document the decision for project consistency

**Framework Usage:**

- **With testify**: Use `assert` and `require` for clear assertions
- **Without testify**: Use standard `t.Error`, `t.Fatal`, and manual comparisons
- **Both cases**: Follow same test organization and naming patterns

### Test Organization

- **File naming**:
  - `<package name>_test.go` - Unit tests for error cases, edge cases and complex golden cases
  - `example_test.go` - Example functions and simple golden cases (godoc) - **MUST use `package <name>_test`** in
    package name declaration for black-box testing from user perspective
  - `<package name>_internal_test.go` - Private function tests
- **Package declaration**:
  - `example_test.go` → **ALWAYS `package <name>_test`** (black-box, external user view)
  - `<package name>_test.go` → `package <name>_test` for public API testing (black-box)
  - `<package name>_internal_test.go` → `package <name>` for private function access (white-box)
- **Location**: Next to code being tested

### Test Naming and Structure

- **Format**: `Test_functionName_scenario`
- **Subtests**: Use `t.Run` for organization
- **Table-driven**: Preferred for multiple scenarios
- **Coverage**: Both success and error cases
- **Framework**: `github.com/stretchr/testify` if available, otherwise standard `testing` package

### Test Assertion Examples

**With testify:**

```go
assert.Equal(t, expected, actual)
require.NoError(t, err)
assert.Contains(t, slice, item)
```

**Without testify (standard library):**

```go
if got != want {
    t.Errorf("got %v, want %v", got, want)
}
if err != nil {
    t.Fatalf("unexpected error: %v", err)
}
// Manual slice checks with loops
```

### Test Helpers

- Mark with `t.Helper()`
- Create fixtures for complex setup
- Use `testing.TB` interface for benchmarks
- Clean up with `t.Cleanup()`

## Type Safety

_Summary: Use strong typing and deliberate pointer choices to prevent invalid states._

- Define types and struct tags to make invalid states unrepresentable.
- Prefer explicit conversions and always check the second return value on type assertions.
- Choose pointer or value receivers intentionally and stay consistent within a type.
- Keep interfaces small, accept interfaces, and return concrete types.

### Type Definitions

- Define types for meaning and safety
- Use struct tags (JSON, XML, DB)
- Prefer explicit conversions
- Check second return value in type assertions
- Use `any` instead of `interface{}` (Go 1.18+)
- Prefer generics over unconstrained types

### Pointers vs Values

**Receivers:**

- Pointer: Large structs or need to modify
- Value: Small structs or immutability desired
- Be consistent within type's method set

**Parameters:**

- Pointer: Need to modify or large structs
- Value: Small structs or prevent modification

**Consider:** Zero value usefulness

### Interface Design Principles

- Accept interfaces, return concrete types
- Keep small (1-3 methods)
- Use embedding for composition
- Define where used, not where implemented
- Don't export unless necessary

## Concurrency

_Summary: Coordinate goroutines carefully and clean up every execution path._

- Let callers control concurrency when possible and provide explicit shutdown paths.
- Use `sync.WaitGroup`, channels, and mutexes appropriately to avoid leaks.
- Close channels from the sender side and keep critical sections small.
- Use modern Go features (e.g., `WaitGroup.Go` in Go 1.25+) when available; provide fallbacks for older versions.

> Note: Use the Go version declared in the project's `go.mod` as the upper bound for examples and patterns in this
> guide. For Go 1.25+ you may use `sync.WaitGroup.Go`; for this repository (go 1.24.0), prefer the classic `Add/Done`
> pattern instead.

### Goroutines

- Be cautious in libraries (let caller control)
- Provide cleanup mechanisms if creating goroutines
- Always know exit path
- Use `sync.WaitGroup` or channels to wait
- Avoid leaks with proper cleanup

### Channels

- Use for goroutine communication
- Share memory by communicating
- Close from sender side, not receiver
- Buffer when capacity known
- Use `select` for non-blocking ops

### Synchronization

- **`sync.Mutex`**: Protecting shared state
- **`sync.RWMutex`**: Many readers scenario
- **`sync.Once`**: One-time initialization
- **Keep critical sections small**
- **Choose wisely**: Channels for communication, mutexes for state

#### WaitGroup by Go Version

```go
// Go 1.25+: Use WaitGroup.Go method
var wg sync.WaitGroup
wg.Go(task1)
wg.Go(task2)
wg.Wait()
```

```go
// Go < 1.25: Classic Add/Done pattern
var wg sync.WaitGroup
wg.Add(1)
go func() {
    defer wg.Done()
    task1()
}()
wg.Wait()
```

## API Design

_Summary: Expose predictable, context-aware APIs with clean HTTP semantics._

- Choose handler types that match complexity and apply middleware for cross-cutting concerns.
- Validate inputs, set appropriate status codes, and return informative errors.
- Prefer per-request HTTP client setup with context support and safe resource cleanup.
- Use Go's versioned router capabilities judiciously, sticking to standard library defaults first.

### HTTP Handlers

- **Simple**: Use `http.HandlerFunc`
- **Stateful**: Implement `http.Handler`
- **Cross-cutting**: Use middleware
- **Responses**: Appropriate status codes and headers
- **Errors**: Handle gracefully

#### Router by Go Version

- **Go 1.22+**: Use enhanced `ServeMux` with pattern-based routing
- **Go < 1.22**: Classic `ServeMux` or justified third-party router

### JSON APIs

- Use struct tags for marshaling control
- Validate input data
- Use pointers for optional fields
- Consider `json.RawMessage` for delayed parsing
- Handle JSON errors appropriately

### HTTP Clients

- **Client struct**: Configuration only (URL, timeouts, auth)
- **No state**: Don't store `*http.Request` in struct
- **Per-request**: Build fresh request each time
- **Context**: Accept `context.Context` in methods
- **Thread-safe**: Ensure `*http.Client` is concurrent-safe
- **Cleanup**: Always close response bodies with `defer`

## Performance

_Summary: Optimize only after measurement, focusing on memory discipline and I/O safety._

- Minimize allocations, reuse buffers thoughtfully, and preallocate when sizes are known.
- Remember that most readers are single-pass and clone data if multiple passes are needed.
- Handle HTTP bodies safely by recreating readers or using `GetBody` for retries.
- Profile with Go's tooling and prioritize algorithmic gains over micro-optimizations.

### Memory Management

- Minimize allocations in hot paths
- Reuse objects with `sync.Pool`
- Use value receivers for small structs
- Preallocate slices when size known
- Avoid unnecessary string conversions

### I/O: Readers and Buffers

#### Key Concepts

- Most `io.Reader` streams are **consumable once**
- Reading advances state - no rewind without special handling
- Buffer data to create multiple readers from same content

#### Multiple Reads Pattern

```go
// Read once, create multiple readers
data, err := io.ReadAll(reader)
if err != nil {
    return err
}

// Create fresh readers as needed
reader1 := bytes.NewReader(data)
reader2 := bytes.NewReader(data)
```

#### HTTP Request Bodies

```go
// Don't reuse consumed body
// Instead, keep original payload
payload := []byte(`{"key":"value"}`)

// Recreate body for each request
req.Body = io.NopCloser(bytes.NewReader(payload))

// Or use GetBody for redirects/retries
req.GetBody = func() (io.ReadCloser, error) {
    return io.NopCloser(bytes.NewReader(payload)), nil
}
```

#### Streaming with io.Pipe

```go
pr, pw := io.Pipe()

// Write in goroutine
go func() {
    defer pw.Close()
    if err := writeData(pw); err != nil {
        pw.CloseWithError(err)
        return
    }
}()

// Use pr as streaming reader
resp, err := http.Post(url, "application/json", pr)
```

**⚠️ Warning**: Writes must be sequential - no concurrent/out-of-order writes!

### Profiling

- Use built-in `pprof` tools
- Benchmark critical paths with `testing.B`
- Profile BEFORE optimizing
- Focus on algorithmic improvements first

## Security Best Practices

_Summary: Treat every input as hostile and rely on proven cryptography and transport._

- Validate and sanitize all external inputs before use in file, query, or shell contexts.
- Prefer strong typing to prevent invalid states and guard against injection attacks.
- Use only standard library cryptography with secure random sources and key handling.
- Protect communications with TLS for network traffic.

### Input Validation

- Validate all external input
- Use strong typing for invalid state prevention
- Sanitize SQL query inputs
- Be careful with user-provided file paths
- Escape data for context (HTML, SQL, shell)

### Cryptography

- Use standard library crypto packages
- **NEVER implement your own crypto**
- Use `crypto/rand` for random numbers
- Hash passwords with bcrypt/scrypt/argon2
- Use TLS for network communication

## Documentation Files

_Summary: Keep user-facing guides in sync with behavior and easy to follow._

- Maintain README sections with setup, usage, configuration, and troubleshooting details.
- Update documentation whenever public APIs, features, or workflows change.
- Ensure examples remain accurate and reproducible for contributors and users.
- Remove outdated or redundant text to avoid misleading the community.

### README Requirements

- Clear setup instructions
- Dependencies and requirements
- Usage examples
- Configuration options
- Troubleshooting section

### Markdown Formatting

- **Header spacing**: Add blank line after header lines for readability
- **List formatting**: Surround lists with blank lines (required for MD032 linting)
- **Code blocks**: Use proper fencing with language specification
- **Line length**: Target 80 characters for readability
- **Consistent style**: Follow project's existing markdown conventions

## Tools and Workflow

_Summary: Use standard Go tooling consistently and keep modules tidy._

- Run `go test`, `go test -race`, and coverage checks regularly during development.
- Enforce linting with `golangci-lint` and formatting via `gofmt` or `goimports`.
- Manage dependencies with `go mod tidy` and `go mod download` when needed.
- Leverage profiling tools only after establishing functional correctness.

### Essential Tools

- `go fmt` - Format code
- `go vet` - Find suspicious constructs
- `golangci-lint` - Comprehensive linting
- `go test` - Run tests
- `go mod` - Manage dependencies
- `goimports` - Auto-manage imports

### Essential Commands

```bash
# Testing
go test -v ./...                   # Run all tests
go test -race ./...                # Check race conditions
go test -cover ./...               # Coverage report
go test -coverprofile coverage.out # Generate coverage file to record coverage
go test -run TestName              # Run specific test

# Linting and formatting
golangci-lint run            # Run linter
gofmt -w .                   # Format code
goimports -w .               # Fix imports

# Dependencies
go mod tidy                  # Clean dependencies
go mod download              # Download dependencies

# Profiling
go test -cpuprofile=cpu.prof
go test -memprofile=mem.prof
```

### Default golangci-lint Configuration

When setting up a new Go project, use this `.golangci.yml` configuration template:

```yaml
version: "2"

run:
  tests: true
  allow-parallel-runners: true

linters:
  default: all
  exclusions:
    paths:
      - .github
  settings:
    depguard:
      rules:
        main:
          allow:
            - $gostd
            - github.com/stretchr/testify  # Only if project uses testify
            - <project package>
  disable:
    # Disabled due to wsl_v5
    - wsl
    # Disabled due to testing private function purpose
    - testpackage
```

**Configuration notes:**

- **`default: all`**: Enables all available linters for maximum code quality
- **`tests: true`**: Lints test files as well as production code
- **`allow-parallel-runners: true`**: Enables concurrent linting for faster execution
- **`exclusions.paths`**: Excludes `.github` directory from linting
- **`depguard`**: Restricts dependencies to standard library, testify (if used), and project packages
- **Disabled linters**:
  - `wsl`: Conflicts with wsl_v5 configuration
  - `testpackage`: Allows testing private functions (white-box testing)

**Important**: Replace `<project package>` with your actual project import path (e.g., `github.com/username/projectname`).

## Common Pitfalls

_Summary: Watch for frequent errors that break the TDD contract or Go best practices._

- Never write production code before tests or skip mandatory TDD steps.
- Always handle errors, run race detection, and clean up unused imports.
- Prevent goroutine leaks, concurrent map access, and nil-interface mistakes.
- Remove temporary files and avoid duplicate package declarations.

1. **Writing production code before tests** → TDD VIOLATION
2. **Skipping TDD steps** → Complete all 10 steps
3. **Not checking errors** → Always use `if err != nil`
4. **Ignoring race conditions** → Run `go test -race`
5. **Unused imports** → Use `goimports -w .`
6. **Goroutine leaks** → Ensure exit paths
7. **Not using defer** → Always defer cleanup
8. **Concurrent map access** → Use mutex or `sync.Map`
9. **Nil interface confusion** → Careful with nil checks
10. **Forgetting to close resources** → Use `defer` for files, connections
11. **Global variables** → Prefer dependency injection
12. **Over-using `any`** → Use specific types or generics
13. **Ignoring zero value** → Design for useful zero values
14. **Duplicate `package` declarations** → ONE per file only

## Debugging Tips

_Summary: Use Go's testing flags and profiling tools to isolate issues quickly._

- Run targeted tests with verbose output to inspect failing scenarios fast.
- Apply race detection proactively to surface concurrency issues early.
- Capture CPU and memory profiles when performance questions arise.
- Use `t.Logf` within tests for lightweight debug information.

```bash
# Test debugging
go test -v                    # Verbose output
go test -run TestName         # Single test
t.Logf("debug: %v", value)   # Debug logging in tests

# Race detection
go test -race ./...          # Detect data races

# Profiling
go test -cpuprofile=cpu.prof
go tool pprof cpu.prof
```

## Decision Reference

_Summary: Quick lookup table for common decision points during development._

| Scenario | Detection Command | Action |
|----------|------------------|--------|
| testify missing | `go list -m all \| grep testify` | Ask user approval |
| Custom errors exist | `go list ./... \| grep errors` | Use custom `errors` package |
| Coverage drops | `compare_coverage $baseline $current` | Add more tests |
| Lint config missing | `[ -f ".golangci.yml" ]` | Use default config |

**Note**: The `compare_coverage` function should be implemented temporarily when needed for coverage comparison.

## Pre-Commit Checklist

_Summary: Confirm every safeguard passes before handing work back._

- Ensure race checks, coverage, and linting are clean.
- Apply `goimports` and remove temporary artifacts.
- Update documentation whenever public behavior changes.
- Re-run targeted example tests to confirm narratives remain valid.

- [ ] `go test -race ./...` passes
- [ ] `go test -cover ./...` stays within the logged baseline
- [ ] `golangci-lint run` shows no issues
- [ ] `goimports -w .` applied
- [ ] `go test -run Example` passes
- [ ] Documentation updated for public API changes
- [ ] Temporary files removed (`coverage.out`, `*.prof`)
- [ ] `go mod tidy` executed

## Agent Restrictions

_Summary: Stay within approved automation limits unless the user explicitly expands scope._

- Avoid deployments, dependency additions, and CI config changes without approval.
- Steer clear of destructive operations such as file deletion or overwriting configs.
- Keep all automation aligned with documented project rules and user directives.
- Escalate uncertainties instead of guessing when policies feel ambiguous.

### Prohibited Without Approval

- Direct deployment operations
- Adding new dependencies
- Modifying `.golangci.yml` or CI config files
- Destructive operations (deleting files, overwriting configs)

## Agent Self-Evaluation

_Summary: Double-check that every policy, quality gate, and cleanup task is complete._

- Validate TDD adherence, coverage, and documentation updates.
- Confirm code quality, error handling, and security posture remain solid.
- Ensure no unauthorized changes or dependencies slipped in.
- Run final cleanups and verify compatibility with public interfaces.

After completing tasks, verify:

- [ ] **TDD adherence**: All 10 steps completed
- [ ] **Test coverage**: Sufficient edge cases and error scenarios
- [ ] **Documentation**: README and godoc updated
- [ ] **Code quality**: Clean, maintainable, no complexity
- [ ] **Error handling**: All errors handled with meaningful messages
- [ ] **Security**: Best practices followed, no vulnerabilities
- [ ] **No unapproved changes**: Dependencies and config files unchanged
- [ ] **Cleanup**: Temporary artifacts removed
- [ ] **CI/CD**: All checks pass (lint, test, format)
- [ ] **Compatibility**: Public interfaces backward-compatible

---

**Remember**: TDD is not optional. It's the PRIMARY development methodology. Tests drive design and implementation.
