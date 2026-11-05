# Architecture Skill — Clean Architecture

## Directory Structure

Every feature follows this structure:

```
lib/
├── core/                        ← Shared infrastructure (never feature-specific)
│   ├── api_service/
│   │   ├── network.dart         ← ApiService (Dio wrapper)
│   │   ├── api_handler.dart     ← executeApiRequest()
│   │   ├── api_response.dart    ← ApiResponse<T> model
│   │   └── interceptor.dart     ← Auth token injection
│   ├── config/
│   │   ├── app_config.dart      ← Abstract config base
│   │   ├── dev_config.dart
│   │   └── prod_config.dart
│   ├── constants/
│   │   ├── app_colors.dart
│   │   └── app_images.dart
│   ├── di/
│   │   └── service_locator.dart ← GetIt setup
│   ├── errors/
│   │   ├── exceptions.dart      ← Exception types
│   │   └── failure.dart         ← Failure types
│   ├── routing/
│   │   ├── routes.dart          ← Routes enum
│   │   └── app_router.dart      ← GoRouter config
│   ├── services/
│   │   └── storage_service.dart ← SharedPreferences
│   ├── session/
│   │   ├── session_cubit.dart
│   │   └── session_state.dart
│   ├── theme/
│   │   ├── app_theme.dart
│   │   ├── theme_extensions.dart
│   │   └── gradient_theme_extension.dart
│   ├── usecase/
│   │   └── usecase.dart         ← UseCase<T, P> base
│   ├── utils/
│   │   └── validators.dart
│   └── app_navigator.dart       ← Static theme/color access
├── features/
│   └── auth/                    ← Example feature
│       ├── domain/
│       │   ├── entities/
│       │   │   └── auth_entity.dart
│       │   ├── repositories/
│       │   │   └── auth_repository.dart     ← Abstract interface
│       │   └── usecases/
│       │       ├── auth_params/
│       │       │   └── auth_request_params.dart
│       │       └── login_usecase.dart
│       ├── data/
│       │   ├── datasources/
│       │   │   ├── auth_endpoints.dart
│       │   │   └── auth_remote_datasource.dart
│       │   ├── models/
│       │   │   └── auth_model.dart          ← Extends entity
│       │   └── repositories/
│       │       └── auth_repository_impl.dart ← Implements abstract
│       ├── presentation/
│       │   ├── cubit/
│       │   │   ├── auth_cubit.dart
│       │   │   └── auth_state.dart
│       │   ├── screens/
│       │   └── widgets/
│       └── auth_injection.dart              ← Feature DI
└── shared/
    └── widgets/                 ← From flutter-kit-widgets
```

---

## Layer Responsibilities

### Domain Layer (pure Dart, no Flutter/external packages)
- **Entities**: Plain data classes with `Equatable`
- **Repository interfaces**: Abstract contracts, not implementations
- **Use cases**: Single-responsibility classes — one use case per operation

### Data Layer
- **Models**: Extend entities, add `fromJson`/`toJson`
- **Remote datasources**: Only API calls — no business logic
- **Repository implementations**: Coordinate datasources + map exceptions to failures

### Presentation Layer
- **Cubit/State**: UI state management
- **Screens**: UI only — read state, dispatch events
- **Widgets**: Reusable UI pieces for this feature

---

## UseCase Pattern

```dart
// Base class
abstract class UseCase<Type, Params> {
  Future<Either<Failure, Type>> call(Params params);
}

// NoParams sentinel
class NoParams extends Equatable {
  @override
  List<Object?> get props => [];
}

// Implementation
class LoginUseCase implements UseCase<AuthEntity, LoginRequestParams> {
  final AuthRepository _repository;
  LoginUseCase(this._repository);

  @override
  Future<Either<Failure, AuthEntity>> call(LoginRequestParams params) =>
      _repository.login(params);
}
```

---

## Model Pattern

```dart
// Entity (domain)
class AuthEntity extends Equatable {
  final String token;
  final Map<String, dynamic> user;
  const AuthEntity({required this.token, required this.user});
  @override
  List<Object?> get props => [token, user];
}

// Model (data) — extends entity
class AuthModel extends AuthEntity {
  const AuthModel({required super.token, required super.user});

  factory AuthModel.fromJson(Map<String, dynamic> json) {
    return AuthModel(
      token: json['token'] ?? json['accessToken'] ?? '',
      user: json['user'] as Map<String, dynamic>? ?? {},
    );
  }
}
```

---

## Repository Pattern

```dart
// Abstract (domain)
abstract class AuthRepository {
  Future<Either<Failure, AuthEntity>> login(LoginRequestParams params);
}

// Implementation (data)
class AuthRepositoryImpl implements AuthRepository {
  final AuthRemoteDatasource _remote;
  AuthRepositoryImpl(this._remote);

  @override
  Future<Either<Failure, AuthEntity>> login(LoginRequestParams params) =>
      executeApiRequest(() async {
        final json = await _remote.login(params);
        return AuthModel.fromJson(json);
      });
}
```
