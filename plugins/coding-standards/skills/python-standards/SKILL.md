---
name: python-standards
description: >
  Enforce Python coding standards for pure functions, type hints, dataclasses,
  Google-style docstrings, and single responsibility. Use this skill when
  writing Python code, reviewing Python, creating Python functions, classes,
  or modules. Triggers on: "python", ".py file", "Python function",
  "Python class", "dataclass", "type hints", "docstring", "Google-style",
  "Python best practices", "Python review", "Python standards",
  "refactor Python", "pytest", "Python module".
allowed-tools: Read, Grep, Glob
---

# Python Coding Standards

## Step 1: Load Reference Examples

Read `${CLAUDE_SKILL_DIR}/references/examples.md` for before/after code samples.

## Core Rules

### 1. Pure Functions and Method Abstraction

- Write functions as **pure functions** whenever possible (no side effects, no modifying external state or input arguments)
- Break complex logic into smaller, well-named private helper methods
- Each function should have a **single, clear responsibility**
- Use `_private_method` naming for internal helpers in classes

### 2. Documentation Standards (Google-style Docstrings)

All functions and methods must be documented with Google-style docstrings. No inline comments explaining what code does — the code should be self-explanatory through good naming and structure.

Required elements:
- Clear summary of the function's purpose
- `Args:` section with parameter descriptions
- `Returns:` section with return description

```python
def filter_data(data: list[DataItem]) -> list[DataItem]:
    """
    Filters a list to include only valid items with id and value > 10.

    Args:
        data: The list of data to filter.

    Returns:
        A new list containing only valid items.
    """
```

### 3. Type Safety

- Use **type hints** for all function parameters and return values
- Use **dataclasses** or typed dicts for structured data — never raw dicts for domain objects
- Use `list[T]`, `dict[K, V]`, `Optional[T]` from modern Python typing (3.10+)
- Avoid `Any` — define proper types

## Application

When generating or reviewing Python code, apply all rules above. Reference the examples for correct patterns. Flag any violations found during review.
