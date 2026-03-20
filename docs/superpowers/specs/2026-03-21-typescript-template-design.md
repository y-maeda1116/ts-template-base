# TypeScript Template Repository Design Document

**Date:** 2026-03-21
**Author:** Claude

## Overview

Create a cross-platform (Windows/Mac) TypeScript template repository optimized for Node.js tool development, with consideration for future web frontend (React) migration.

## Requirements

- Support both Windows and Mac platforms
- Optimized for Node.js tool development
- Future-proof for React migration
- Include CI/CD for test and build verification

## Project Structure

```
ts-template-base/
├── src/
│   ├── index.ts              # Entry point with signal handling
│   ├── config/
│   │   └── index.ts          # Environment variable validation
│   ├── lib/                  # Common logic (placeholder)
│   └── index.test.ts         # Sample Vitest test
├── .github/workflows/
│   └── ci.yml                # CI workflow (test + build)
├── .env.example              # Environment variable examples
├── .gitignore                # Exclusions for Node, Windows, Mac
├── package.json
├── tsconfig.json
├── tsup.config.ts            # Build configuration
├── .eslintrc.cjs             # ESLint configuration
├── .prettierrc               # Prettier configuration
└── vitest.config.ts          # Vitest configuration
```

## Technology Stack

| Category | Tool | Version |
|----------|------|---------|
| Runtime | Node.js | 20.x |
| Package Manager | npm | - |
| Build Tool | tsup | ^8.3.5 |
| Dev Runtime | tsx | ^4.19.2 |
| TypeScript | typescript | ^5.7.3 |
| Linting | eslint | ^9.20.1 |
| Formatting | prettier | ^3.4.2 |
| Testing | vitest | ^2.1.8 |
| Validation | zod | ^3.24.1 |
| Config | dotenv | ^16.4.5 |

## package.json Scripts

| Script | Command | Purpose |
|--------|---------|---------|
| `dev` | `tsx watch src/index.ts` | Hot-reload development |
| `build` | `tsup src/index.ts --clean --format cjs,esm` | Build production files |
| `start` | `node dist/index.js` | Run built files |
| `test` | `vitest run` | Run tests |
| `test:watch` | `vitest` | Watch mode testing |
| `lint` | `eslint src/**` | Lint source code |
| `format` | `prettier --write src/**` | Format source code |

## Implementation Details

### TypeScript Configuration

```json
{
  "compilerOptions": {
    "target": "ESNext",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "outDir": "./dist"
  }
}
```

### Signal Handling (src/index.ts)

The entry point includes graceful shutdown handling:
- Listens for `SIGINT` (Ctrl+C)
- Listens for `SIGTERM` (kill command)
- Logs shutdown message before exit

### Environment Variable Validation (src/config/index.ts)

Uses Zod schema for robust validation:
```typescript
const envSchema = z.object({
  DISCORD_TOKEN: z.string().min(1),
  DEEPL_AUTH_KEY: z.string().min(1),
});
```

### Sample Test (src/index.test.ts)

Simple Vitest test demonstrating framework setup:
- Test `Math` addition functionality
- Target pattern: `src/**/*.test.ts`

## CI/CD Pipeline

The GitHub Actions workflow:

1. **Test Job**
   - Runs on: Ubuntu, Windows, macOS
   - Node.js version: 20.x
   - Steps: checkout, setup node, npm ci, npm run test, npm run lint

2. **Build Job**
   - Runs on: Ubuntu (after test passes)
   - Steps: checkout, setup node, npm ci, npm run build

## Cross-Platform Considerations

- Line endings: Git handles LF/CRLF conversion automatically
- Path separators: Use `path.join()` for file paths
- Signal handling: Both SIGINT and SIGTERM for Unix, Ctrl+C for Windows
- Build output: CommonJS and ESM formats for compatibility

## Success Criteria

- [ ] Project structure matches specification
- [ ] All npm scripts work correctly
- [ ] TypeScript compilation passes with strict mode
- [ ] ESLint and Prettier configured and working
- [ ] Vitest runs sample test successfully
- [ ] tsup builds both CJS and ESM formats
- [ ] CI workflow passes on all platforms
- [ ] Environment variable validation works
- [ ] Signal handling works gracefully
