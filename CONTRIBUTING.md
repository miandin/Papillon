# Contributing to Papillon AI

Thanks for helping Papillon AI evolve. Contributions are welcome: bug fixes, UX polish, docs, and new features.

## Ways to contribute
- File bugs and feature requests (use the templates)
- Improve docs (README/CHANGELOG)
- Submit pull requests (PRs)

## Development setup 
1) Clone the repo and install dependencies.
   - If the repo has `pnpm-lock.yaml`, use `pnpm install`
   - Otherwise use `npm install`

2) Build the extension (commands may vary by repo scripts):
   - `pnpm run build` or `npm run build`

3) Run locally in VS Code:
   - Open the extension project in VS Code
   - Press `F5` to launch the Extension Development Host

## Packaging a VSIX
- `npx vsce package`
- Install locally:
  - VS Code → Extensions → `...` → Install from VSIX…
  - or `code --install-extension <file>.vsix`

## PR guidelines
- Keep changes **additive** and avoid breaking existing behavior.
- Any new feature should be optional and gated by:
  - a new command, or
  - a new setting (default OFF).
- Update docs if behavior/UI changes:
  - README (Overview)
  - CHANGELOG (Changes)
- Include screenshots for UX changes.

## PR checklist
- [ ] No breaking changes
- [ ] Builds successfully on macOS
- [ ] New settings default to OFF
- [ ] Updated README/CHANGELOG if needed
- [ ] Added/updated tests if applicable

## Code style
- Prefer small, focused commits
- Use clear command and setting names (`papillon.*`)
