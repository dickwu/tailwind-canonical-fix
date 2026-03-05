# tailwind-canonical-fix

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that auto-fixes non-canonical Tailwind CSS class names across your project.

Tailwind CSS IntelliSense warns about non-canonical classes (e.g. `h-[18px]` can be written as `h-4.5`), but offers no batch-fix. This skill fixes them all in one go.

## Examples

| Before | After |
|--------|-------|
| `h-[18px]` | `h-4.5` |
| `min-h-[400px]` | `min-h-100` |
| `py-[2px]` | `py-0.5` |
| `w-[600px]` | `w-150` |
| `flex-grow` | `grow` |

## Installation

Copy the `SKILL.md` file into your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills/tailwind-canonical-fix
cp SKILL.md ~/.claude/skills/tailwind-canonical-fix/
```

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI
- Tailwind CSS v4 project
- A CSS entry point (e.g. `src/app/globals.css`)

The skill will automatically install [`@laststance/tailwind-suggest-canonical-classes`](https://www.npmjs.com/package/@laststance/tailwind-suggest-canonical-classes) as a dev dependency if not already present.

## Usage

In Claude Code, trigger the skill by saying:

- "fix tailwind canonical classes"
- "run tailwind canonical fix"
- "suggestCanonicalClasses auto fix"

The skill will:

1. Install the CLI tool if needed
2. Add a `fix-tailwind` script to your `package.json` for easy reuse
3. Detect your CSS entry point (`src/app/globals.css`)
4. Run a dry-run to preview all changes
5. Apply fixes across all `.tsx`, `.jsx`, and `.ts` files
6. Run Prettier to maintain formatting
7. Show a `git diff --stat` summary

After the first run, you can re-run the fix anytime without Claude Code:

```bash
bun run fix-tailwind
```

### Options

- **Target specific files**: Ask Claude to target a specific directory (e.g. "fix canonical classes in `src/components/staff-chat/`")
- **Dry run only**: Ask Claude to only preview changes without applying
- **Custom root font size**: Specify if your project uses a non-default root font size

## How It Works

Uses [`@laststance/tailwind-suggest-canonical-classes`](https://github.com/laststance/tailwindcss-canonical-classes-monrepo) which reads your Tailwind v4 design system directly from your CSS entry point and replaces arbitrary values with their canonical theme equivalents.

Supports class names in:
- JSX `className` attributes
- `cn()`, `clsx()`, `classNames()`, `twMerge()`, `cva()` utility functions
- String literals and template literals

## License

MIT
