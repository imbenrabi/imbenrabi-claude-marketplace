---
name: typescript-standards
description: >
  Enforce TypeScript coding standards for pure functions, proper types, TSDoc,
  and single responsibility. Use this skill when writing TypeScript code,
  reviewing TypeScript, creating TypeScript functions, classes, or interfaces.
  Triggers on: "typescript", ".ts file", ".tsx file", "TypeScript function",
  "TypeScript class", "TypeScript interface", "TSDoc", "type safety",
  "no any", "proper types", "TypeScript best practices", "TypeScript review",
  "TypeScript standards", "refactor TypeScript".
allowed-tools: Read, Grep, Glob
---

# TypeScript Coding Standards

## Step 1: Load Reference Examples

Read `${CLAUDE_SKILL_DIR}/references/examples.md` for before/after code samples.

## Core Rules

### 1. Pure Functions and Method Abstraction

- Write functions as **pure functions** whenever possible (no side effects, no modifying external state or input arguments)
- Break complex logic into smaller, well-named private helper methods
- Each function should have a **single, clear responsibility**
- In classes, use `#privateMethod` for helpers. In modules, use unexported functions

### 2. Documentation Standards (TSDoc)

All functions and methods must be documented with TSDoc. No inline comments explaining what code does — the code should be self-explanatory through good naming and structure.

Required elements:
- Clear summary of the function's purpose
- `@param` tag for each parameter
- `@returns` tag for the return value

```ts
/**
 * Filters a list to include only valid items with id and value > 10.
 * @param data - The array of data to filter.
 * @returns A new array containing only valid items.
 */
```

### 3. Type Safety

- **Never use `any`**. Define proper interfaces or types for all data structures
- Prefer `unknown` over `any` when the type is truly unknown, then narrow with type guards
- Use `readonly` for data that should not be mutated
- Prefer `interface` for object shapes, `type` for unions/intersections

## Application

When generating or reviewing TypeScript code, apply all rules above. Reference the examples for correct patterns. Flag any violations found during review.
