---
name: rust-standards
description: >
  Enforce Rust coding standards for proper struct types, Rustdoc comments,
  ownership and borrowing, Result/Option error handling, derive macros,
  and single responsibility. Use this skill when writing Rust code,
  reviewing Rust, creating Rust functions, structs, or modules.
  Triggers on: "rust", "rustlang", ".rs file", "Rust function",
  "Rust struct", "Rust trait", "Rustdoc", "ownership", "borrowing",
  "Result", "Option", "Rust best practices", "Rust review",
  "Rust standards", "refactor Rust", "cargo", "Rust module",
  "derive", "impl block", "lifetime", "Rust error handling".
allowed-tools: Read, Grep, Glob
---

# Rust Coding Standards

## Step 1: Load Reference Examples

Read `${CLAUDE_SKILL_DIR}/references/examples.md` for before/after code samples.

## Core Rules

### 1. Pure Functions and Ownership

- Write functions as **pure functions** whenever possible — take inputs by reference, return new values
- Break complex logic into smaller, well-named private helper functions
- Each function should have a **single, clear responsibility**
- Prefer borrowing (`&T`) over ownership transfer unless the function needs to consume the value
- Use iterators and combinators (`.filter()`, `.map()`, `.collect()`) over manual loops

### 2. Documentation Standards (Rustdoc)

All public functions, types, and methods must have Rustdoc comments (`///`). No inline comments explaining what code does — the code should be self-explanatory through good naming and structure.

Required elements:
- Summary line describing the function's purpose
- `# Arguments` section for parameters (when not self-evident)
- `# Returns` section for return value

```rust
/// Filters a slice to include only items with a non-empty ID and value greater than 10.
///
/// # Arguments
///
/// * `data` - The slice of data items to filter.
///
/// # Returns
///
/// A new vector containing only the valid items.
fn filter_data(data: &[DataItem]) -> Vec<DataItem> {
```

### 3. Type Safety

- **Never use `Box<dyn Any>`** when a concrete type or generic is available
- Define proper structs for all domain data with `#[derive(Debug, Clone)]` at minimum
- Use enums for variants instead of string constants or magic numbers
- Use newtypes for domain concepts (e.g., `struct UserId(String)`)
- Prefer generics over trait objects when the type is known at compile time

### 4. Error Handling

- Use `Result<T, E>` for operations that can fail — never `unwrap()` or `expect()` in library code
- Define custom error types or use `thiserror` for library errors
- Use `anyhow` for application-level errors
- Use the `?` operator for error propagation
- `Option<T>` for values that may be absent — not sentinel values

### 5. Idiomatic Patterns

- Derive standard traits: `Debug`, `Clone`, `PartialEq` as appropriate
- Implement `Display` for types that are user-facing
- Use `impl` blocks to keep methods with their types
- Prefer `&str` over `String` in function parameters
- Use `Into<T>` / `AsRef<T>` for flexible APIs

## Application

When generating or reviewing Rust code, apply all rules above. Reference the examples for correct patterns. Flag any violations found during review.
