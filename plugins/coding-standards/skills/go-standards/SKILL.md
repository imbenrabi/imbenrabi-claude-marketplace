---
name: go-standards
description: >
  Enforce Go coding standards for proper struct types, GoDoc comments, receiver
  methods, value semantics, error handling, and single responsibility. Use this
  skill when writing Go code, reviewing Go, creating Go functions, structs,
  or packages. Triggers on: "go", "golang", ".go file", "Go function",
  "Go struct", "Go interface", "GoDoc", "receiver method", "value semantics",
  "Go best practices", "Go review", "Go standards", "refactor Go",
  "Go package", "Go module", "goroutine", "Go error handling".
allowed-tools: Read, Grep, Glob
---

# Go Coding Standards

## Step 1: Load Reference Examples

Read `${CLAUDE_SKILL_DIR}/references/examples.md` for before/after code samples.

## Core Rules

### 1. Pure Functions and Method Abstraction

- Write functions as **pure functions** whenever possible (no side effects, no modifying external state or input arguments)
- Break complex logic into smaller, well-named unexported helper functions
- Each function should have a **single, clear responsibility**
- Prefer value receivers unless mutation is necessary
- Return new values rather than mutating inputs

### 2. Documentation Standards (GoDoc)

All exported functions, types, and methods must have GoDoc comments. No inline comments explaining what code does — the code should be self-explanatory through good naming and structure.

Required format:
- Start the comment with the name of the element being documented
- Use complete sentences

```go
// FilterData returns a new slice containing only items with a non-empty
// ID and a Value greater than 10.
func FilterData(data []DataItem) []DataItem {
```

### 3. Type Safety

- **Never use `interface{}` or `any`** when a concrete type is available
- Define proper structs for all domain data
- Use custom types for domain concepts (e.g., `type UserID string`)
- Use named return values only when they improve clarity

### 4. Error Handling

- Always check and handle errors — never ignore them with `_`
- Return errors rather than panicking
- Wrap errors with context: `fmt.Errorf("doing X: %w", err)`
- Use sentinel errors or custom error types for errors callers need to distinguish

### 5. Naming Conventions

- **MixedCaps** (exported) and **mixedCaps** (unexported), never underscores
- Short receiver names (1-2 letters matching the type initial)
- Package names: lowercase, single word when possible, no underscores
- Acronyms are all caps: `HTTP`, `ID`, `URL`

## Application

When generating or reviewing Go code, apply all rules above. Reference the examples for correct patterns. Flag any violations found during review.
