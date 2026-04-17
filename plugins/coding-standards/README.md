# coding-standards

Auto-triggering skills that enforce consistent coding standards across TypeScript, Python, Go, and Rust.

## How It Works

These skills activate automatically based on conversation context. When you write or review code in a supported language, the matching skill loads and guides code generation to follow the standards.

Skills are read-only — they guide output, they do not modify files.

## Standards Enforced

All languages share three core principles:

1. **Pure functions** — no side effects, no modifying external state or input arguments. Single responsibility per function. Complex logic broken into small, well-named helpers.
2. **Proper types** — no `any` (TS), no untyped dicts (Python), no `interface{}` (Go), no `Box<dyn Any>` (Rust). Define explicit types for all data structures.
3. **Documentation** — every exported/public function documented. No inline comments explaining what code does — the code should be self-explanatory through naming.

Language-specific documentation conventions:

| Language | Convention | Required elements |
|----------|------------|-------------------|
| TypeScript | TSDoc | `@param`, `@returns` |
| Python | Google-style docstrings | `Args:`, `Returns:` |
| Go | GoDoc comments | Start with function name |
| Rust | Rustdoc (`///`) | Summary, `# Arguments`, `# Returns` |

## Skills

| Skill | Triggers on |
|-------|-------------|
| `typescript-standards` | `.ts`, `.tsx`, TypeScript classes/interfaces/functions |
| `python-standards` | `.py`, Python classes/functions, dataclasses, type hints |
| `go-standards` | `.go`, Go structs/interfaces, goroutines, error handling |
| `rust-standards` | `.rs`, Rust structs/traits, ownership, `Result`/`Option` |

Each skill includes a `references/examples.md` with before/after code samples.
