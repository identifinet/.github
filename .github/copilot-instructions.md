# Copilot Agent Instructions — Identifi Organisation

You are a coding agent working within the **Identifi** organisation's .NET microservice ecosystem.
Follow these rules precisely on every task you are assigned.

---

## Technology Stack

- **Runtime:** .NET 9 (C# 13), ASP.NET Core Web API
- **Containerisation:** Docker — Alpine-based multi-stage images with non-root user
- **Testing:** xUnit, coverlet for code coverage
- **CI:** GitHub Actions — all PRs must pass the `Unit Tests & Code Coverage` workflow

---

## Code Standards

- Use **minimal, idiomatic C#** — prefer expression-bodied members, pattern matching, and primary constructors where they aid clarity.
- All public types and members **must have XML documentation comments** (`<summary>`, `<param>`, `<response>` etc.).
- Controllers must use `[ProducesResponseType]` on every action.
- Keep controllers thin — business logic belongs in services, not controllers.
- Enable **nullable reference types** (`<Nullable>enable</Nullable>`) and resolve all warnings.
- Never suppress warnings with `#pragma` without an explanatory comment.

---

## Testing Requirements ⚠️ CRITICAL

> **Every pull request must achieve a minimum of 99% line code coverage with all tests passing.**

Enforcement rules:

1. **Write tests before or alongside every code change** — no code ships without tests.
2. Every new controller action requires tests covering **all response-code branches** (200, 201, 204, 400, 404, etc.).
3. Every new service/helper method requires unit tests for **happy path and all error/edge cases**.
4. Use `IDisposable` test fixtures to **isolate state** — static or shared state must be reset between tests.
5. Run the full test suite (`dotnet test --settings coverage.runsettings --collect:"XPlat Code Coverage"`) before opening a PR and confirm:
   - All tests pass (`Failed: 0`)
   - Line coverage ≥ 99%
6. Do **not** use `[Fact(Skip = ...)]` or comment out assertions to inflate coverage.
7. Test names must follow the convention: `MethodName_Condition_ExpectedResult`.

---

## Pull Request Checklist

Before marking a PR ready for review, verify:

- [ ] `dotnet build` succeeds with zero errors and zero warnings
- [ ] `dotnet test` — all tests pass, line coverage ≥ 99%
- [ ] New endpoints are documented in Swagger (XML doc comments present)
- [ ] Dockerfile still builds if `src/` was modified
- [ ] No secrets, connection strings, or credentials committed

---

## Commit Message Format

Use the **Conventional Commits** format:

```
<type>(<scope>): <short description>

[optional body]

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

Types: `feat`, `fix`, `test`, `refactor`, `docs`, `chore`, `ci`

---

## Project Structure Conventions

```
src/
  Controllers/   # Thin API controllers only
  Models/        # Request/response/domain models
  Services/      # Business logic (add when needed)
tests/
  Controllers/   # Mirror of src/Controllers
  Models/        # Mirror of src/Models
  Services/      # Mirror of src/Services
.github/
  workflows/     # CI workflows + copilot-setup-steps.yml
```

---

## What NOT to Do

- Do not push directly to `master` — always use a feature branch and open a PR.
- Do not merge a PR with failing checks or coverage below 99%.
- Do not add packages without justification in the PR description.
- Do not use `Thread.Sleep` or `Task.Delay` in tests — use deterministic approaches.
