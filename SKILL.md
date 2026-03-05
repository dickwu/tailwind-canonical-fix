---
name: tailwind-canonical-fix
description: Auto-fix Tailwind CSS non-canonical class names across the project. Use when the user mentions tailwind canonical classes, suggestCanonicalClasses, fix tailwind classes, canonical fix, or wants to batch-fix Tailwind CSS class warnings like "h-[18px] can be written as h-4.5".
---

# Tailwind Canonical Classes Auto-Fix

Automatically fixes non-canonical Tailwind CSS class names (e.g. `h-[18px]` → `h-4.5`, `flex-grow` → `grow`) across all `.tsx`, `.jsx`, and `.ts` files using `@laststance/tailwind-suggest-canonical-classes`.

## Prerequisites

This skill requires the `@laststance/tailwind-suggest-canonical-classes` package. Install it if not present:

```bash
bun add -D @laststance/tailwind-suggest-canonical-classes
```

## Workflow

### Step 1: Verify installation

Check if `@laststance/tailwind-suggest-canonical-classes` is in `package.json` devDependencies. If not, install it:

```bash
bun add -D @laststance/tailwind-suggest-canonical-classes
```

### Step 2: Add convenience script to package.json

Check the project's `package.json` for an existing `fix-tailwind` script. If not present, add it so the user can easily re-run the fix later:

```bash
npm pkg set scripts.fix-tailwind='tailwind-suggest-canonical-classes "src/**/*.{tsx,ts,jsx,js}" --css ./src/app/globals.css'
```

This allows the user to re-run the fix anytime with:

```bash
bun run fix-tailwind
```

### Step 3: Detect CSS entry point

The CSS entry point is `src/app/globals.css` (standard across admin, front, tool projects). Verify the file exists before proceeding.

### Step 4: Dry run (preview changes)

Run a check-only pass to show what would change without modifying files:

```bash
bunx tailwind-suggest-canonical-classes "src/**/*.{tsx,jsx,ts}" --css ./src/app/globals.css --check --verbose
```

Show the user a summary of what will be changed. If no changes are found, inform the user and stop.

### Step 5: Apply fixes

After showing the dry run results, apply the fixes:

```bash
bunx tailwind-suggest-canonical-classes "src/**/*.{tsx,jsx,ts}" --css ./src/app/globals.css --verbose
```

### Step 6: Format changed files

Run prettier on the modified files to ensure consistent formatting:

```bash
bunx prettier --write "src/**/*.{tsx,jsx,ts}"
```

### Step 7: Show summary

Run `git diff --stat` to show the user a summary of all changed files. Optionally show a few example diffs with `git diff` on specific files to illustrate the transformations.

## Options

- **Target specific directory**: Replace `"src/**/*.{tsx,jsx,ts}"` with a specific path like `"src/components/staff-chat/**/*.tsx"`
- **Root font size**: Add `--root-font-size 16` if your project uses a non-default root font size
- **Dry run only**: If the user only wants to see what would change, stop after Step 4

## Notes

- This tool reads your Tailwind v4 design system directly from the CSS entry point
- Only classes that have a shorter/cleaner canonical equivalent are changed
- The tool handles class attributes, `cn()`, `clsx()`, `classNames()`, template literals, and JSX expressions
- Always review `git diff` after applying to verify changes are correct
