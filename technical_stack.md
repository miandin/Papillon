# Papillon AI - Technical Stack & Development Guide

## 🛠️ Technical Stack

### Core Technologies
- **Node.js**: 20.19.2 (specified in `.nvmrc`)
- **TypeScript**: 5.4.5 (type-safe JavaScript)
- **VS Code Extension API**: Latest stable

### Package Management
- **pnpm**: 10.8.1 (fast, disk-efficient package manager)
- **Monorepo**: pnpm workspaces for multi-package management

### Build System
- **Turborepo**: 2.5.6 (high-performance build system)
- **esbuild**: 0.25.0 (fast JavaScript bundler)
- **Vite**: For webview UI bundling

### Frontend (Webview UI)
- **React**: UI component library
- **TypeScript**: Type-safe React components
- **CSS**: Styling

### Extension Development
- **VS Code Extension API**: Core extension functionality
- **TypeScript**: Extension source code
- **Node.js APIs**: File system, process management

### Code Quality
- **ESLint**: 9.27.0 (code linting)
- **Prettier**: 3.4.2 (code formatting)
- **Husky**: 9.1.7 (git hooks)
- **lint-staged**: 16.0.0 (pre-commit checks)

### Version Management
- **Changesets**: 2.27.10 (version and changelog management)

### Packaging
- **@vscode/vsce**: 3.3.2 (VS Code extension packager)

---

## 📦 Packaging & Release Approach

### 1. Build Process
```bash
pnpm build
```
- Compiles TypeScript to JavaScript
- Bundles webview UI with Vite
- Processes all packages in monorepo
- Output: Compiled files in `dist/` and `out/` directories

### 2. Package to .vsix
```bash
pnpm vsix
```
- Uses `@vscode/vsce` to package extension
- Bundles all compiled code and assets
- Creates `.vsix` file in `bin/` directory
- Output: `papillon-ai-v2.vsix` (41 MB)

### 3. Release Strategy
- **GitHub Releases**: Upload `.vsix` as release asset
- **Version Tags**: Use semantic versioning (v2.0.0)
- **Changelog**: Auto-generated from changesets
- **Distribution**: Users download `.vsix` from GitHub Releases

---

## 🔄 Development Sequence

### Phase 1: Setup (One-time)
1. **Clone Repository**
   ```bash
   git clone https://github.com/YOUR-USERNAME/Papillon.git
   cd Papillon
   ```

2. **Install Dependencies**
   ```bash
   pnpm install
   ```
   - Installs all packages from `pnpm-lock.yaml`
   - Sets up monorepo workspaces
   - Installs dev tools (ESLint, Prettier, etc.)

### Phase 2: Development (Iterative)
3. **Make Code Changes**
   - Edit TypeScript files in `src/`
   - Edit React components in `webview-ui/src/`
   - Edit shared packages in `packages/`

4. **Test Changes**
   ```bash
   pnpm test
   ```
   - Runs unit tests
   - Validates code changes

5. **Format & Lint**
   ```bash
   pnpm format
   pnpm lint
   ```
   - Auto-formats code with Prettier
   - Checks code quality with ESLint

### Phase 3: Build (Before Release)
6. **Build Extension**
   ```bash
   pnpm build
   ```
   - Compiles all TypeScript
   - Bundles webview UI
   - Processes all packages
   - Takes 5-10 minutes

7. **Package Extension**
   ```bash
   pnpm vsix
   ```
   - Creates `.vsix` file
   - Output: `bin/papillon-ai-v2.vsix`

### Phase 4: Release (Distribution)
8. **Test Installation**
   ```bash
   code --install-extension bin/papillon-ai-v2.vsix
   ```
   - Install locally to test
   - Verify all features work

9. **Create GitHub Release**
   - Tag version: `v2.0.0`
   - Upload `.vsix` file
   - Add release notes
   - Publish release

10. **Users Install**
    ```bash
    # Download from GitHub Releases
    code --install-extension papillon-ai-v2.vsix
    ```

---

## 📁 Key Files & Their Purpose

### Configuration
- **`package.json`**: Main package manifest, scripts, dependencies
- **`pnpm-workspace.yaml`**: Monorepo workspace configuration
- **`tsconfig.json`**: TypeScript compiler settings
- **`turbo.json`**: Build pipeline configuration

### Source Code
- **`src/`**: Extension TypeScript source code
- **`webview-ui/`**: React UI components
- **`packages/`**: Shared TypeScript packages

### Build Output (Not in Git)
- **`dist/`**: Compiled JavaScript
- **`out/`**: Extension output
- **`bin/`**: Packaged `.vsix` files

---

## 🔧 Common Commands

### Development
```bash
pnpm install          # Install dependencies
pnpm build            # Build extension
pnpm test             # Run tests
pnpm lint             # Check code quality
pnpm format           # Format code
```

### Packaging
```bash
pnpm vsix             # Create .vsix package
pnpm clean            # Clean build artifacts
```

### Installation
```bash
code --install-extension bin/papillon-ai-v2.vsix
```

---

## 🎯 Simple Development Flow

```
1. Clone repo
   ↓
2. pnpm install
   ↓
3. Make changes to code
   ↓
4. pnpm build
   ↓
5. pnpm vsix
   ↓
6. Test .vsix locally
   ↓
7. Create GitHub Release
   ↓
8. Users download & install
```

---

## 📊 Build Time Estimates

- **First Install**: 5-10 minutes (downloading dependencies)
- **Build**: 3-5 minutes (compiling TypeScript, bundling UI)
- **Package**: 30-60 seconds (creating .vsix)
- **Total**: ~10-15 minutes for first build

---

## 🚀 Quick Start for New Developers

```bash
# 1. Clone
git clone https://github.com/YOUR-USERNAME/Papillon.git
cd Papillon

# 2. Install (requires Node 20.19.2 and pnpm)
pnpm install

# 3. Build
pnpm build

# 4. Package
pnpm vsix

# 5. Install locally
code --install-extension bin/papillon-ai-v2.vsix
```

---

## 📝 Summary

**Stack**: Node.js + TypeScript + React + VS Code API  
**Build**: Turborepo + esbuild + Vite  
**Package**: @vscode/vsce → .vsix file  
**Release**: GitHub Releases  
**Install**: VS Code CLI or UI  

Simple, efficient, and production-ready! 🎉
