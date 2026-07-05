# CLAUDE.md

Guidance for AI assistants (and humans) working in this repository.

## Current state

This repository is **newly initialized and effectively empty**. As of this
writing it contains only:

- `README.md` — a title-only file (`# CircleSquareTriangle`).
- `CLAUDE.md` — this file.

There is **no source code, build system, dependency manifest, test suite, or
CI configuration yet**. Do not assume a language, framework, or toolchain — none
has been chosen. Any statement below about "how things are done" is a
convention to establish, not one already in place.

## What this means for you

When asked to add code:

1. **Confirm the intent before scaffolding.** The project name suggests shapes
   (circle, square, triangle) — possibly a graphics, geometry, game, or
   teaching project — but the direction is unstated. Ask the user what they want
   to build and in which language/framework before creating structure.
2. **Establish, then document.** Whatever stack gets chosen (language, package
   manager, test runner, formatter), record the concrete commands in this file
   as soon as they exist, replacing the placeholders below.
3. **Keep this file honest.** Update `CLAUDE.md` in the same change that
   introduces a new tool or convention, so it never drifts from reality.

## Repository layout

```
.
├── README.md    # Project title
└── CLAUDE.md    # This file
```

Update this tree as directories and files are added.

## Development workflow

### Branching

- Default branch: `main`.
- Do work on feature branches; do not commit directly to `main`.
- Use clear, descriptive commit messages.

### Build / test / lint

_No commands exist yet._ Once a toolchain is chosen, document the real commands
here, for example:

```
# install dependencies
# <command>

# run the app
# <command>

# run tests
# <command>

# lint / format
# <command>
```

Until then, there is nothing to build or test.

## Conventions

No project-specific conventions have been established. When you introduce the
first ones (directory structure, naming, formatting, commit style), write them
down in this section so future work stays consistent.

## Notes

- Keep changes small and focused.
- Prefer well-known, conventional project layouts for whatever stack is chosen,
  so contributors and tools can navigate the code without surprises.
