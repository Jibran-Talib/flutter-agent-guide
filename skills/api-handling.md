# API Handling Skill

## executeApiRequest

The single wrapper for ALL API calls. Located at `core/api_service/api_handler.dart`.

```dart
Future<Either<Failure, T>> executeApiRequest<T>(
  Future<T> Function() request,
) async {
  try {
    final result = await request();
    return Right(result);
  } on ServerException catch (e) {
    return Left(ServerFailure(e.message));
  } on NetworkException catch (e) {
    return Left(NetworkFailure(e.message));
  } on UnauthorizedException catch (e) {
    return Left(UnauthorizedFailure(e.message));
  } catch (e) {
    return Left(ServerFailure(e.toString()));
  }
}
```

**ALWAYS wrap repository calls with this. Never throw from a repository.**

---

## ApiService Usage

```dart
final _apiService = sl<ApiService>();

// GET
final response = await _apiService.get('/users/me');

// POST
final response = await _apiService.post('/auth/login', data: params.toJson());

// PATCH
final response = await _apiService.patch('/users/me', data: body);

// DELETE
await _apiService.delete('/posts/$id');

// File upload (multipart)
final response = await _apiService.post(
  '/profile/photo',
  data: FormData.fromMap({'image': await MultipartFile.fromFile(path)}),
);
```

---

## Repository Implementation Pattern

```dart
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

---

## Datasource Pattern

Datasources only call the API and return raw maps. Never handle exceptions here — let `executeApiRequest` catch them.

```dart
class AuthRemoteDatasource {
  final ApiService _apiService;
  AuthRemoteDatasource(this._apiService);

  Future<Map<String, dynamic>> login(LoginRequestParams params) async {
    final response = await _apiService.post(
      AuthEndpoints.login,
      data: params.toJson(),
    );
    return response.data as Map<String, dynamic>;
  }
}
```

---

## ApiResponse (Paginated)

For list endpoints that return pagination metadata:

```dart
class ApiResponse<T> {
  final List<T> data;
  final int total;
  final int page;
  final int limit;
  final bool hasNextPage;

  const ApiResponse({
    required this.data,
    required this.total,
    required this.page,
    required this.limit,
    required this.hasNextPage,
  });

  factory ApiResponse.fromJson(
    Map<String, dynamic> json,
    T Function(Map<String, dynamic>) fromJson,
  ) {
    return ApiResponse(
      data: (json['data'] as List).map((e) => fromJson(e)).toList(),
      total: json['total'] ?? 0,
      page: json['page'] ?? 1,
      limit: json['limit'] ?? 10,
      hasNextPage: json['hasNextPage'] ?? false,
    );
  }
}
```

---

## Endpoints Pattern

```dart
class AuthEndpoints {
  static const String _base = '/auth';
  static const String login = '$_base/login';
  static const String signup = '$_base/signup';
  static const String logout = '$_base/logout';
  static const String verifyOtp = '$_base/verify-otp';
  static const String resendOtp = '$_base/resend-otp';
  static const String forgotPassword = '$_base/forgot-password';
  static const String resetPassword = '$_base/reset-password';
}
```

---

## Handling the Either in Cubits

```dart
final result = await sl<LoginUseCase>().call(params);

result.fold(
  (failure) => emit(state.copyWith(
    status: AuthStatus.failure,
    message: failure.message,
  )),
  (auth) {
    sl<SessionCubit>().setSession(token: auth.token, user: auth.user);
    emit(state.copyWith(status: AuthStatus.success));
  },
);
```
