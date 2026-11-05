# Core Services Catalog — flutter-kit-core

Download the entire core layer, or individual files as needed.

**Base URL:** `https://raw.githubusercontent.com/Jibran-Talib/flutter-kit-core/main`

> TODO: Replace `Jibran-Talib` with the actual GitHub username after pushing the repo.

---

## Download All Core Files

```bash
BASE="https://raw.githubusercontent.com/Jibran-Talib/flutter-kit-core/main"

# API Service
curl -o lib/core/api_service/network.dart "$BASE/lib/core/api_service/network.dart"
curl -o lib/core/api_service/api_handler.dart "$BASE/lib/core/api_service/api_handler.dart"
curl -o lib/core/api_service/api_response.dart "$BASE/lib/core/api_service/api_response.dart"
curl -o lib/core/api_service/interceptor.dart "$BASE/lib/core/api_service/interceptor.dart"

# Errors
curl -o lib/core/errors/exceptions.dart "$BASE/lib/core/errors/exceptions.dart"
curl -o lib/core/errors/failure.dart "$BASE/lib/core/errors/failure.dart"

# Config
curl -o lib/core/config/app_config.dart "$BASE/lib/core/config/app_config.dart"
curl -o lib/core/config/dev_config.dart "$BASE/lib/core/config/dev_config.dart"
curl -o lib/core/config/prod_config.dart "$BASE/lib/core/config/prod_config.dart"

# Constants
curl -o lib/core/constants/app_colors.dart "$BASE/lib/core/constants/app_colors.dart"
curl -o lib/core/constants/app_images.dart "$BASE/lib/core/constants/app_images.dart"

# Theme
curl -o lib/core/theme/app_theme.dart "$BASE/lib/core/theme/app_theme.dart"
curl -o lib/core/theme/theme_extensions.dart "$BASE/lib/core/theme/theme_extensions.dart"
curl -o lib/core/theme/gradient_theme_extension.dart "$BASE/lib/core/theme/gradient_theme_extension.dart"

# Services
curl -o lib/core/services/storage_service.dart "$BASE/lib/core/services/storage_service.dart"

# Session
curl -o lib/core/session/session_cubit.dart "$BASE/lib/core/session/session_cubit.dart"
curl -o lib/core/session/session_state.dart "$BASE/lib/core/session/session_state.dart"

# DI
curl -o lib/core/di/service_locator.dart "$BASE/lib/core/di/service_locator.dart"

# UseCase
curl -o lib/core/usecase/usecase.dart "$BASE/lib/core/usecase/usecase.dart"

# Routing
curl -o lib/core/routing/routes.dart "$BASE/lib/core/routing/routes.dart"
curl -o lib/core/routing/app_router.dart "$BASE/lib/core/routing/app_router.dart"

# Utils
curl -o lib/core/utils/validators.dart "$BASE/lib/core/utils/validators.dart"
curl -o lib/core/app_navigator.dart "$BASE/lib/core/app_navigator.dart"
```

---

## Individual Service Files

### ApiService (network.dart)
Dio wrapper with all HTTP methods.
```bash
curl -o lib/core/api_service/network.dart \
  "https://raw.githubusercontent.com/Jibran-Talib/flutter-kit-core/main/lib/core/api_service/network.dart"
```

### executeApiRequest (api_handler.dart)
Wraps any async call in `Either<Failure, T>`.
```bash
curl -o lib/core/api_service/api_handler.dart \
  "https://raw.githubusercontent.com/Jibran-Talib/flutter-kit-core/main/lib/core/api_service/api_handler.dart"
```

### StorageService
SharedPreferences singleton for auth token, device info, FCM token.
```bash
curl -o lib/core/services/storage_service.dart \
  "https://raw.githubusercontent.com/Jibran-Talib/flutter-kit-core/main/lib/core/services/storage_service.dart"
```

### SessionCubit
Generic session management with `setSession()`, `updateUser()`, `logout()`.
```bash
curl -o lib/core/session/session_cubit.dart \
  "https://raw.githubusercontent.com/Jibran-Talib/flutter-kit-core/main/lib/core/session/session_cubit.dart"
curl -o lib/core/session/session_state.dart \
  "https://raw.githubusercontent.com/Jibran-Talib/flutter-kit-core/main/lib/core/session/session_state.dart"
```

### Validators
Form validators: email, password, phone, username, OTP, confirmPassword.
```bash
curl -o lib/core/utils/validators.dart \
  "https://raw.githubusercontent.com/Jibran-Talib/flutter-kit-core/main/lib/core/utils/validators.dart"
```
