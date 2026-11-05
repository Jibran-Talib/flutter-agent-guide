# State Management Skill — Bloc/Cubit

## Cubit vs Bloc

- **Cubit**: Simple UI state — loading, success, failure. No complex event mapping needed.
- **Bloc**: Complex event-driven logic — multiple input types, event transformations, debounce.

Use Cubit by default. Use Bloc only when events have different logic paths.

---

## State Class Pattern

```dart
// Enum for action tracking
enum AuthAction { login, signup, verifyOtp, forgotPassword }

// State class
class AuthState extends Equatable {
  final AuthStatus status;
  final AuthAction? action;
  final String? message;

  const AuthState({
    this.status = AuthStatus.initial,
    this.action,
    this.message,
  });

  // Helper: is this action loading?
  bool isLoadingOf(AuthAction a) => status == AuthStatus.loading && action == a;

  AuthState copyWith({
    AuthStatus? status,
    AuthAction? action,
    String? message,
  }) => AuthState(
    status: status ?? this.status,
    action: action ?? this.action,
    message: message ?? this.message,
  );

  @override
  List<Object?> get props => [status, action, message];
}
```

---

## Cubit Pattern

```dart
class AuthCubit extends Cubit<AuthState> {
  final LoginUseCase _loginUseCase;
  final SessionCubit _sessionCubit;

  AuthCubit({
    required LoginUseCase loginUseCase,
    required SessionCubit sessionCubit,
  })  : _loginUseCase = loginUseCase,
        _sessionCubit = sessionCubit,
        super(const AuthState());

  Future<void> login(LoginRequestParams params) async {
    emit(state.copyWith(status: AuthStatus.loading, action: AuthAction.login));

    final result = await _loginUseCase.call(params);

    result.fold(
      (failure) => emit(state.copyWith(
        status: AuthStatus.failure,
        message: failure.message,
      )),
      (auth) {
        _sessionCubit.setSession(token: auth.token, user: auth.user);
        emit(state.copyWith(status: AuthStatus.success));
      },
    );
  }
}
```

---

## Status Enum Pattern

```dart
enum AuthStatus { initial, loading, success, failure }
```

---

## In the UI

```dart
// BlocBuilder — rebuild on state change
BlocBuilder<AuthCubit, AuthState>(
  builder: (context, state) {
    return ButtonComponent.primary(
      text: 'Login',
      isLoading: state.isLoadingOf(AuthAction.login),
      onPressed: () => context.read<AuthCubit>().login(params),
    );
  },
)

// BlocListener — side effects only
BlocListener<AuthCubit, AuthState>(
  listener: (context, state) {
    if (state.status == AuthStatus.failure) {
      CustomSnackBar.error(context, state.message ?? 'Error');
    }
    if (state.status == AuthStatus.success) {
      context.go(Routes.home.path);
    }
  },
  child: ...,
)

// BlocConsumer — both
BlocConsumer<AuthCubit, AuthState>(
  listener: (context, state) { ... },
  builder: (context, state) { ... },
)
```

---

## Providing Cubits

```dart
// Single cubit
BlocProvider(
  create: (_) => sl<AuthCubit>(),
  child: LoginScreen(),
)

// Multiple cubits
MultiBlocProvider(
  providers: [
    BlocProvider(create: (_) => sl<AuthCubit>()),
    BlocProvider(create: (_) => sl<ProfileCubit>()),
  ],
  child: HomeScreen(),
)
```

---

## SessionCubit (Global)

Provided at the app root. Used by all features that need auth state.

```dart
// Set session after login
sl<SessionCubit>().setSession(token: auth.token, user: auth.user);

// Update user data
sl<SessionCubit>().updateUser({'isPremium': true});

// Logout
sl<SessionCubit>().logout();

// Read current user
final user = sl<SessionCubit>().state.user;  // Map<String, dynamic>
```
