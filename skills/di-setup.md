# DI Setup Skill — GetIt

## Service Locator Setup

The root DI file is `core/di/service_locator.dart`.

```dart
final sl = GetIt.instance;

Future<void> initDependencies() async {
  // Core
  await _initCore();

  // Features (call each feature's injection function)
  initAuthInjection();
  initIapInjection(onPurchaseSuccess: (productId) {
    // unlock premium
  });
}

Future<void> _initCore() async {
  // Storage
  final prefs = await SharedPreferences.getInstance();
  sl.registerLazySingleton<StorageService>(() => StorageService(prefs));

  // API
  sl.registerLazySingleton<ApiService>(() => ApiService(
    baseUrl: AppConfig.instance.baseUrl,
    storage: sl<StorageService>(),
  ));

  // Session
  sl.registerLazySingleton<SessionCubit>(() => SessionCubit(sl<StorageService>()));
}
```

---

## Feature Injection Pattern

Each feature has its own `*_injection.dart`:

```dart
void initAuthInjection({SocialTokenProvider? socialTokenProvider}) {
  // Datasources
  sl.registerLazySingleton<AuthRemoteDatasource>(
    () => AuthRemoteDatasource(sl<ApiService>()),
  );

  // Repositories
  sl.registerLazySingleton<AuthRepository>(
    () => AuthRepositoryImpl(
      sl<AuthRemoteDatasource>(),
      socialTokenProvider: socialTokenProvider,
    ),
  );

  // Use Cases
  sl.registerLazySingleton(() => LoginUseCase(sl<AuthRepository>()));
  sl.registerLazySingleton(() => SignupUseCase(sl<AuthRepository>()));
  // ... other use cases

  // Cubit (Factory — new instance per screen)
  sl.registerFactory<AuthCubit>(() => AuthCubit(
    loginUseCase: sl<LoginUseCase>(),
    signupUseCase: sl<SignupUseCase>(),
    sessionCubit: sl<SessionCubit>(),
  ));
}
```

---

## Registration Types

| Method | Use For | Lifecycle |
|--------|---------|-----------|
| `registerLazySingleton` | Services, repos, use cases | Created once, reused |
| `registerSingleton` | Same but created immediately | Created at registration |
| `registerFactory` | Cubits | New instance every time |
| `registerFactoryParam` | Cubits that need a param (e.g., post ID) | New instance with param |

---

## Accessing in Code

```dart
// Get a singleton
final apiService = sl<ApiService>();

// Get a new cubit instance (in BlocProvider)
BlocProvider(
  create: (_) => sl<AuthCubit>(),
  child: LoginScreen(),
)

// Get with parameter
BlocProvider(
  create: (_) => sl.get<PostDetailCubit>(param1: postId),
  child: PostDetailScreen(),
)
```

---

## App Entry Point

```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await initDependencies();  // Must complete before runApp
  runApp(
    BlocProvider.value(
      value: sl<SessionCubit>(),  // Global, lives for app lifetime
      child: const MyApp(),
    ),
  );
}
```

---

## Checklist When Adding a New Feature

1. Create `lib/features/feature_name/feature_name_injection.dart`
2. Register: datasource → repository → use cases → cubit (in that order)
3. Import and call `initFeatureNameInjection()` from `service_locator.dart`
4. Use `sl.registerLazySingleton` for datasource/repo/use cases
5. Use `sl.registerFactory` for cubit
