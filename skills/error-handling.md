# Error Handling Skill

## Exception Types (core/errors/exceptions.dart)

Thrown by datasources. Never surfaced to the UI.

```dart
class ServerException implements Exception {
  final String message;
  const ServerException([this.message = 'Server error occurred']);
}

class NetworkException implements Exception {
  final String message;
  const NetworkException([this.message = 'No internet connection']);
}

class UnauthorizedException implements Exception {
  final String message;
  const UnauthorizedException([this.message = 'Unauthorized']);
}

class CacheException implements Exception {
  final String message;
  const CacheException([this.message = 'Cache error occurred']);
}
```

---

## Failure Types (core/errors/failure.dart)

Returned from repositories via `Either<Failure, T>`. These reach the UI.

```dart
abstract class Failure extends Equatable {
  final String message;
  const Failure(this.message);
  @override
  List<Object?> get props => [message];
}

class ServerFailure extends Failure {
  const ServerFailure([super.message = 'Server error occurred']);
}

class NetworkFailure extends Failure {
  const NetworkFailure([super.message = 'No internet connection']);
}

class UnauthorizedFailure extends Failure {
  const UnauthorizedFailure([super.message = 'Session expired']);
}

class CacheFailure extends Failure {
  const CacheFailure([super.message = 'Cache error occurred']);
}
```

---

## Exception → Failure Mapping

Inside `executeApiRequest` in `api_handler.dart`:

```dart
Future<Either<Failure, T>> executeApiRequest<T>(
  Future<T> Function() request,
) async {
  try {
    return Right(await request());
  } on ServerException catch (e) {
    return Left(ServerFailure(e.message));
  } on NetworkException catch (e) {
    return Left(NetworkFailure(e.message));
  } on UnauthorizedException catch (e) {
    return Left(UnauthorizedFailure(e.message));
  } on CacheException catch (e) {
    return Left(CacheFailure(e.message));
  } catch (e) {
    return Left(ServerFailure(e.toString()));
  }
}
```

---

## Throwing Exceptions in Datasources

```dart
Future<Map<String, dynamic>> login(LoginRequestParams params) async {
  try {
    final response = await _apiService.post(AuthEndpoints.login, data: params.toJson());
    if (response.statusCode == 200 || response.statusCode == 201) {
      return response.data as Map<String, dynamic>;
    }
    throw ServerException(response.data['message'] ?? 'Login failed');
  } on DioException catch (e) {
    if (e.type == DioExceptionType.connectionError) {
      throw const NetworkException();
    }
    if (e.response?.statusCode == 401) {
      throw UnauthorizedException(e.response?.data['message'] ?? 'Unauthorized');
    }
    throw ServerException(e.response?.data['message'] ?? 'Server error');
  }
}
```

---

## Interceptor (Automatic Token + 401 Handling)

The `interceptor.dart` in `api_service/` handles:
- Injecting `Authorization: Bearer <token>` on all requests
- On 401: triggering `SessionCubit.logout()` to clear session and redirect to login

---

## Showing Errors in UI

```dart
// Snackbar (non-blocking)
if (state.status == AuthStatus.failure) {
  CustomSnackBar.error(context, state.message ?? 'Something went wrong');
}

// ResponseWidget (inline, below the form)
if (state.status == AuthStatus.failure)
  ResponseWidget.error(title: state.message ?? 'Login failed'),

// Dialog (blocking)
if (state.status == AuthStatus.failure)
  AppDialog.warning(
    title: 'Error',
    message: state.message ?? 'Please try again',
  );
```

---

## TextField Validation Errors

Always use `Validators` from `core/utils/validators.dart`:

```dart
InputField(
  labelText: 'Email',
  controller: _emailController,
  validator: Validators.validateEmail,  // Returns String? error message
)
```

Validators return `null` on valid input, an error string on invalid.
Form uses `AutovalidateMode.onUserInteraction` to show errors as the user types.
