# Repository Guidelines

## Project Structure & Module Organization
- `Scarb.toml` defines the Cairo package; `src/lib.cairo` wires modules exposed on-chain.
- Core abstractions live in `src/base`, `src/interfaces`, `src/utils`, while feature domains sit under `src/component/*` with `contribution`, `transfer`, `user_management`, etc.
- Each component folder groups implementation (`*/*.cairo`), mocks, and focused unit tests in adjacent `test.cairo`; contract-level flows reside in `tests/test_starkremit_factory.cairo`.
- The `cloakpay/` package mirrors patterns used by the main contract—treat it as a reference implementation and keep shared types aligned.

## Build, Test, and Development Commands
- `scarb build`: compile the StarkRemit contract and generate Sierra artifacts in `target/dev`.
- `snforge test`: execute the full Starknet Foundry suite (component tests under `src/component/**/test.cairo` plus integration tests).
- `snforge test --fork SEPOLIA_LATEST`: run fork-aware scenarios using the RPC specified in `Scarb.toml`.
- `scarb fmt`: format Cairo sources; run before committing to enforce consistent spacing and ordering.

## Coding Style & Naming Conventions
- Use 4-space indentation and Cairo snake_case for modules/functions; structs, enums, and traits stay in PascalCase (`RemittanceRecord`, `IStarkRemitDispatcher`).
- Keep constants uppercase (`OWNER`, `ORACLE_ADDRESS`) as seen in `tests/test_starkremit_factory.cairo`.
- Prefer small, composable modules; expose only via `src/lib.cairo`.
- Always run `scarb fmt` after edits; avoid manual reordering of `use` statements.

## Testing Guidelines
- Write unit tests beside the feature (`src/component/<domain>/test.cairo`) with `#[test] fn test_*()` naming.
- Integration or deployment flows belong in `tests/` and should spy on events using `snforge_std::spy_events`.
- Include edge cases for access control, currency handling, and lifecycle transitions; reuse provided mocks before creating new fixtures.
- Verify the suite locally with `snforge test` and attach relevant excerpts from the report when discussing failures.

## Commit & Pull Request Guidelines
- Follow the existing Conventional Commit style (`feat:`, `fix:`, `fmt`, etc.) visible in `git log`.
- Limit commits to cohesive changes and describe client-visible impact in the subject line.
- PRs should summarize behaviour changes, list test commands run, and link issues (e.g., `Closes #123`); include screenshots or calldata snippets for agent-facing flows when applicable.
