# Naming Conventions Skill

## File Names
- All lowercase with underscores: `auth_cubit.dart`, `login_screen.dart`
- Feature injection: `auth_injection.dart`
- Endpoints: `auth_endpoints.dart`
- Models suffix `_model`: `auth_model.dart`
- Entities suffix `_entity`: `auth_entity.dart`
- Cubits suffix `_cubit`: `auth_cubit.dart`
- States suffix `_state`: `auth_state.dart`
- Use cases suffix `_usecase`: `login_usecase.dart`
- Screens suffix `_screen`: `login_screen.dart`

## Class Names
- PascalCase: `AuthCubit`, `LoginUseCase`, `AuthModel`
- Cubits end with `Cubit`: `AuthCubit`, `ProfileCubit`
- States end with `State`: `AuthState`, `ProfileState`
- Entities end with `Entity`: `AuthEntity`, `UserEntity`
- Models end with `Model`: `AuthModel`, `UserModel`
- Use cases end with `UseCase`: `LoginUseCase`, `GetProfileUseCase`
- Screens end with `Screen`: `LoginScreen`, `HomeScreen`

## Variables/Parameters
- camelCase: `authCubit`, `sessionState`, `isLoading`
- Private fields prefixed with `_`: `_apiService`, `_sessionCubit`
- Boolean: prefix `is`/`has`/`can`: `isLoading`, `hasError`, `canRetry`

## Enums
- PascalCase type name: `AuthStatus`, `AuthAction`
- camelCase values: `AuthStatus.loading`, `AuthAction.login`

## Constants
- ALL_CAPS for static const strings: not used here — prefer camelCase in Dart
- Use static const in a class: `AuthEndpoints.login`, `AppColors.primary`

## Parameters Style
- Named parameters for constructors with 2+ args
- Use `required` for non-nullable named params

```dart
// ✅ Correct
AuthCubit({
  required LoginUseCase loginUseCase,
  required SessionCubit sessionCubit,
});

// ❌ Avoid positional for 3+ params
AuthCubit(this._login, this._signup, this._session);
```

## Imports Order
1. Dart core: `dart:async`
2. Flutter: `package:flutter/`
3. Pub packages: `package:flutter_bloc/`, `package:get_it/`
4. Local (relative): `../../core/app_navigator.dart`
