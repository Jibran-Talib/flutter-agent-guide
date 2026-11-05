# Tech Rules — Flutter Kit System

> Read this file before starting ANY task. It contains all repository links and global rules.

---

## Repository Links

### flutter-kit-core
**GitHub:** `https://github.com/Jibran-Talib/flutter-kit-core`
**Raw base:** `https://raw.githubusercontent.com/Jibran-Talib/flutter-kit-core/main`
**Purpose:** Core infrastructure — API, DI, theme, session, storage, routing, errors, validators

| File | Path |
|------|------|
| ApiService | `lib/core/api_service/network.dart` |
| executeApiRequest | `lib/core/api_service/api_handler.dart` |
| ApiResponse | `lib/core/api_service/api_response.dart` |
| Interceptor | `lib/core/api_service/interceptor.dart` |
| AppConfig | `lib/core/config/app_config.dart` |
| DevConfig | `lib/core/config/dev_config.dart` |
| ProdConfig | `lib/core/config/prod_config.dart` |
| AppColors | `lib/core/constants/app_colors.dart` |
| AppImages | `lib/core/constants/app_images.dart` |
| AppTheme | `lib/core/theme/app_theme.dart` |
| ThemeExtensions | `lib/core/theme/theme_extensions.dart` |
| GradientThemeExtension | `lib/core/theme/gradient_theme_extension.dart` |
| Exceptions | `lib/core/errors/exceptions.dart` |
| Failure | `lib/core/errors/failure.dart` |
| StorageService | `lib/core/services/storage_service.dart` |
| SessionCubit | `lib/core/session/session_cubit.dart` |
| SessionState | `lib/core/session/session_state.dart` |
| ServiceLocator | `lib/core/di/service_locator.dart` |
| UseCase | `lib/core/usecase/usecase.dart` |
| Routes | `lib/core/routing/routes.dart` |
| AppRouter | `lib/core/routing/app_router.dart` |
| Validators | `lib/core/utils/validators.dart` |
| AppNavigator | `lib/core/app_navigator.dart` |

---

### flutter-kit-widgets
**GitHub:** `https://github.com/Jibran-Talib/flutter-kit-widgets`
**Raw base:** `https://raw.githubusercontent.com/Jibran-Talib/flutter-kit-widgets/main`
**Purpose:** Reusable UI widgets — individually downloadable
**Full catalog:** `catalogs/widgets.md`

| Widget | Path |
|--------|------|
| ButtonComponent | `lib/shared/widgets/button_component.dart` |
| InputField | `lib/shared/widgets/text_field_component.dart` |
| ResponseWidget | `lib/shared/widgets/response_widget.dart` |
| AppLoader | `lib/shared/widgets/app_loader.dart` |
| LoaderOverlay | `lib/shared/widgets/loader_overlay.dart` |
| CustomSnackBar | `lib/shared/widgets/custom_snackbar.dart` |
| CachedImageComponent | `lib/shared/widgets/cache_image_component.dart` |
| TextComponent | `lib/shared/widgets/text_component.dart` |
| AppDialog | `lib/shared/widgets/dialogs_widget/app_dialog.dart` |
| DialogTypes | `lib/shared/widgets/dialogs_widget/dialog_types.dart` |

---

### flutter-kit-features
**GitHub:** `https://github.com/Jibran-Talib/flutter-kit-features`
**Raw base:** `https://raw.githubusercontent.com/Jibran-Talib/flutter-kit-features/main`
**Purpose:** Complete feature packages — download and connect
**Full catalog:** `catalogs/features.md`

| Feature | Base Path |
|---------|-----------|
| Auth | `auth/lib/` |
| IAP | `iap/lib/` |
| Auth + User (combined) | `auth_user/lib/` |
| ChatBot | `chatbot/lib/` |
| Remote Config (Firebase) | `remote_config/lib/` |

---

### flutter-agent-guide (this repo)
**GitHub:** `https://github.com/Jibran-Talib/flutter-agent-guide`
**Purpose:** Agent rules, skills, and catalogs

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Universal agent guide (all agents) |
| `tech-rules.md` | This file — repo links and global rules |
| `skills/architecture.md` | Clean Architecture patterns |
| `skills/api-handling.md` | executeApiRequest, ApiService patterns |
| `skills/state-management.md` | Bloc/Cubit patterns |
| `skills/error-handling.md` | Exception/Failure hierarchy |
| `skills/di-setup.md` | GetIt patterns |
| `skills/routing.md` | go_router patterns |
| `skills/naming-conventions.md` | Dart/Flutter naming |
| `skills/ui-components.md` | Widget usage |
| `skills/textfield-validation.md` | Validators patterns |
| `skills/pagination.md` | Paginated list patterns |
| `catalogs/widgets.md` | Widget curl download commands |
| `catalogs/services.md` | Core services curl download commands |
| `catalogs/features.md` | Feature curl download commands |

---

## Global Technology Rules

| Technology | Package | Usage |
|------------|---------|-------|
| State Management | `flutter_bloc` | Cubit (simple) + Bloc (complex events) |
| DI | `get_it` | `sl` instance, lazy singletons + factories |
| Navigation | `go_router` | `Routes` enum + `context.go()` / `context.push()` |
| HTTP | `dio` | Via `ApiService` only — never create Dio directly |
| Either | `fpdart` | `Either<Failure, T>` for all repository returns |
| Storage | `shared_preferences` | Via `StorageService` only |
| Images | `cached_network_image` | Via `CachedImageComponent` |
| Responsive | `flutter_screenutil` | `12.sp`, `16.w`, `20.h` |
| Animations | `flutter_animate` | `.animate().fadeIn()` etc. |
| Icons | `iconly` | `IclyBold.`, `IclyLight.` |
| Equality | `equatable` | All entities, states, params |

---

## Rules That CANNOT Be Broken

1. Every API call wrapped in `executeApiRequest` — returns `Either<Failure, T>`
2. No `package:tailored/` or `package:projectname/` in shared code — relative imports only
3. No hardcoded colors — always `AppColors.*`, `context.colors.*`, or `AppNavigator.colors.*`
4. No direct `Navigator.push` — always `context.go()` or `context.push()` with `Routes` enum
5. No raw `SharedPreferences` access — always `StorageService`
6. No business logic in UI (screens/widgets) — only cubits
7. No network calls in domain layer — only data layer
8. README.md must be updated when features are added or changed

---

## HOW_TO_USE.md Files

| Kit | Location |
|-----|----------|
| flutter-kit-core | `flutter-kit-core/HOW_TO_USE.md` |
| flutter-kit-widgets | `flutter-kit-widgets/HOW_TO_USE.md` |
| Auth feature | `flutter-kit-features/auth/HOW_TO_USE.md` |
| IAP feature | `flutter-kit-features/iap/HOW_TO_USE.md` |
| Auth + User feature | `flutter-kit-features/auth_user/HOW_TO_USE.md` |
| ChatBot feature | `flutter-kit-features/chatbot/HOW_TO_USE.md` |
| Remote Config feature | `flutter-kit-features/remote_config/HOW_TO_USE.md` |

> TODO: Replace `Jibran-Talib` with actual GitHub username after pushing all repos.
