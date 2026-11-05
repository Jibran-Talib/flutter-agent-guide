# Flutter Agent Guide

> This file is for ALL AI agents: Claude, ChatGPT, Cursor, Windsurf, Copilot, and others.
> Read this file at project start and ALWAYS follow its rules.

---

## 🚨 CRITICAL RULES — NEVER VIOLATE

1. **NEVER write fresh code** — always use the established patterns from the kit repos below.
2. **NEVER invent architecture** — follow Clean Architecture as documented in `skills/architecture.md`.
3. **NEVER use different state management** — only `flutter_bloc` (Cubit + Bloc).
4. **NEVER use different DI** — only `get_it` via the `sl` instance.
5. **NEVER use different navigation** — only `go_router` with the `Routes` enum.
6. **NEVER hardcode colors or styles** — use `AppNavigator.colors`, `context.colors`, `AppColors`.
7. **NEVER create API calls without `executeApiRequest`** — wrap every API call with it.
8. **NEVER return raw exceptions** — always map to `Either<Failure, T>`.
9. **ALWAYS use relative imports** in core/shared code — not `package:projectname/`.
10. **ALWAYS read `tech-rules.md`** before starting any task.

---

## 📦 Kit Repositories

All reusable code lives in these repos. Download only what you need.

| Repo | Purpose | Catalog |
|------|---------|---------|
| `flutter-kit-core` | Core infrastructure (API, DI, theme, session, storage, routing) | `catalogs/services.md` |
| `flutter-kit-widgets` | Reusable UI widgets (buttons, fields, dialogs, snackbars) | `catalogs/widgets.md` |
| `flutter-kit-features` | Complete features (auth, IAP) | `catalogs/features.md` |

**Always check catalogs first.** The catalogs contain direct curl download commands.

---

## 🏗️ Project Architecture

Every project follows Clean Architecture with this structure:

```
lib/
├── core/                    ← From flutter-kit-core (copy once, do not modify)
│   ├── api_service/
│   ├── config/
│   ├── constants/
│   ├── di/
│   ├── errors/
│   ├── routing/
│   ├── services/
│   ├── session/
│   ├── theme/
│   ├── usecase/
│   └── utils/
├── features/
│   └── feature_name/        ← One folder per feature
│       ├── domain/
│       │   ├── entities/
│       │   ├── repositories/   ← Abstract interfaces only
│       │   └── usecases/
│       ├── data/
│       │   ├── datasources/
│       │   ├── models/
│       │   └── repositories/   ← Implementations
│       └── presentation/
│           ├── cubit/
│           ├── screens/
│           └── widgets/
└── shared/
    └── widgets/             ← From flutter-kit-widgets
```

---

## 🔧 Key Technical Rules

### API Calls
- Always use `executeApiRequest<T>()` from `api_handler.dart`
- Returns `Either<Failure, T>` — never throw directly in repositories
- Use `sl<ApiService>()` — never create Dio instances manually

### State Management
- Use `Cubit` for simple feature state, `Bloc` for complex event-driven flows
- State classes use `copyWith` and `Equatable`
- Action helpers pattern: `state.isLoadingOf(SomeAction.fetch)`

### Dependency Injection
- Register all dependencies in feature's `*_injection.dart` file
- Call that from `service_locator.dart`'s `initDependencies()`
- Use `sl.registerLazySingleton` for services, `sl.registerFactory` for cubits

### Navigation
- Use `Routes` enum + `go_router`
- Navigate with `context.go(Routes.home.path)` or `context.push(Routes.detail.path)`
- Never use `Navigator.pushNamed` or string routes

### Errors
- All exceptions defined in `core/errors/exceptions.dart`
- All failures defined in `core/errors/failure.dart`
- Match exception type to failure type in `api_handler.dart`

---

## 📋 Before Starting Any Task

1. Read `tech-rules.md` — it has all repo links
2. Check `catalogs/` to see what's already built
3. Download only what this task needs (curl commands in catalogs)
4. Read the HOW_TO_USE.md of anything you download
5. Follow existing patterns exactly — don't reinvent

---

## 📝 README Maintenance

Every project must maintain `README.md` with:
- **Project Name** and brief description
- **Version** (semantic, e.g., `1.0.0`)
- **Last Updated** date
- **Architecture** overview
- **Features** list with status
- **Flow** description (how data flows through the app)
- **Setup** instructions
- **API base URL** (dev, staging, prod)

Update README whenever you add a feature or change the architecture.

---

## 🌐 Skills Reference

For detailed patterns on any topic, read the skills files:

| Skill | File |
|-------|------|
| Architecture | `skills/architecture.md` |
| State Management | `skills/state-management.md` |
| API Handling | `skills/api-handling.md` |
| Error Handling | `skills/error-handling.md` |
| DI Setup | `skills/di-setup.md` |
| Routing | `skills/routing.md` |
| Naming Conventions | `skills/naming-conventions.md` |
| UI Components | `skills/ui-components.md` |
| TextField Validation | `skills/textfield-validation.md` |
| Pagination | `skills/pagination.md` |
