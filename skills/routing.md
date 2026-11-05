# Routing Skill — go_router

## Routes Enum

All routes defined in `core/routing/routes.dart`:

```dart
enum Routes {
  splash('/'),
  login('/login'),
  signup('/signup'),
  verifyOtp('/verify-otp'),
  forgotPassword('/forgot-password'),
  home('/home'),
  profile('/profile'),
  settings('/settings');

  const Routes(this.path);
  final String path;
}
```

---

## AppRouter Setup

Located at `core/routing/app_router.dart`:

```dart
class AppRouter {
  static final navigatorKey = GlobalKey<NavigatorState>();

  static GoRouter router(SessionState sessionState) => GoRouter(
    navigatorKey: navigatorKey,
    initialLocation: Routes.splash.path,
    redirect: (context, state) {
      final isLoggedIn = sessionState.isLoggedIn;
      final isAuthRoute = state.matchedLocation == Routes.login.path ||
                          state.matchedLocation == Routes.signup.path;

      if (!isLoggedIn && !isAuthRoute) return Routes.login.path;
      if (isLoggedIn && isAuthRoute) return Routes.home.path;
      return null;
    },
    routes: [
      GoRoute(path: Routes.splash.path, builder: (_, __) => const SplashScreen()),
      GoRoute(path: Routes.login.path, builder: (_, __) => const LoginScreen()),
      GoRoute(path: Routes.home.path, builder: (_, __) => const HomeScreen()),
      // Routes with params:
      GoRoute(
        path: '/posts/:id',
        builder: (_, state) => PostDetailScreen(id: state.pathParameters['id']!),
      ),
    ],
  );
}
```

---

## Usage in Screens

```dart
// Navigate and replace current route
context.go(Routes.home.path);

// Push onto stack
context.push(Routes.profile.path);

// Push with path params
context.push('/posts/$postId');

// Push with query params
context.push('${Routes.search.path}?q=flutter');

// Go back
context.pop();

// Go back with result
context.pop(true);
```

---

## Providing the Router

```dart
// In MaterialApp
MaterialApp.router(
  routerConfig: AppRouter.router(sessionState),
  ...
)

// Or with BlocBuilder for reactive redirect
BlocBuilder<SessionCubit, SessionState>(
  builder: (context, sessionState) => MaterialApp.router(
    routerConfig: AppRouter.router(sessionState),
  ),
)
```

---

## Dialogs and Sheets via AppNavigator

For dialogs that need context-free access:

```dart
// Using navigator key
AppNavigator.showDialog(
  barrierDismissible: false,
  builder: (_) => const SomeDialog(),
);

// In AppDialog static methods
AppDialog.logout(onConfirm: () => sl<SessionCubit>().logout());
```
