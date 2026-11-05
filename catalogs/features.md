# Features Catalog — flutter-kit-features

Complete, downloadable features with Clean Architecture (domain + data + presentation).

**Base URL:** `https://raw.githubusercontent.com/YOUR_USERNAME/flutter-kit-features/main`

> TODO: Replace `YOUR_USERNAME` with the actual GitHub username after pushing the repo.

---

## Available Features

### Auth Feature
Complete authentication: login, signup, OTP, forgot/reset password, social login (Google/Apple), logout.

**Requires:** flutter-kit-core (api_service, errors, session, storage, usecase, di)

```bash
BASE="https://raw.githubusercontent.com/YOUR_USERNAME/flutter-kit-features/main/auth/lib"

# Domain
curl -o lib/features/auth/domain/entities/auth_entity.dart "$BASE/domain/entities/auth_entity.dart"
curl -o lib/features/auth/domain/repositories/auth_repository.dart "$BASE/domain/repositories/auth_repository.dart"
curl -o lib/features/auth/domain/usecases/auth_params/auth_request_params.dart "$BASE/domain/usecases/auth_params/auth_request_params.dart"
curl -o lib/features/auth/domain/usecases/login_usecase.dart "$BASE/domain/usecases/login_usecase.dart"

# Data
curl -o lib/features/auth/data/datasources/auth_endpoints.dart "$BASE/data/datasources/auth_endpoints.dart"
curl -o lib/features/auth/data/datasources/auth_remote_datasource.dart "$BASE/data/datasources/auth_remote_datasource.dart"
curl -o lib/features/auth/data/models/auth_model.dart "$BASE/data/models/auth_model.dart"
curl -o lib/features/auth/data/repositories/auth_repository_impl.dart "$BASE/data/repositories/auth_repository_impl.dart"

# Presentation
curl -o lib/features/auth/presentation/cubit/auth_cubit.dart "$BASE/presentation/cubit/auth_cubit.dart"
curl -o lib/features/auth/presentation/cubit/auth_state.dart "$BASE/presentation/cubit/auth_state.dart"

# Injection
curl -o lib/features/auth/auth_injection.dart "$BASE/auth_injection.dart"
```

**After download:** Read `flutter-kit-features/auth/HOW_TO_USE.md`

**Configure:**
1. Edit `auth_endpoints.dart` → set your API paths
2. Call `initAuthInjection()` from `service_locator.dart`
3. Provide `AuthCubit` to widget tree

---

### IAP Feature (In-App Purchases)
Complete IAP with backend verification: load plans, buy, restore, verify, complete.

**Requires:** flutter-kit-core + `in_app_purchase` package

```bash
BASE="https://raw.githubusercontent.com/YOUR_USERNAME/flutter-kit-features/main/iap/lib"

# Constants & endpoints
curl -o lib/features/iap/constants.dart "$BASE/constants.dart"
curl -o lib/features/iap/iap_endpoints.dart "$BASE/iap_endpoints.dart"

# Domain
curl -o lib/features/iap/domain/entities/in_app_product_entity.dart "$BASE/domain/entities/in_app_product_entity.dart"
curl -o lib/features/iap/domain/entities/in_app_purchase_update_entity.dart "$BASE/domain/entities/in_app_purchase_update_entity.dart"
curl -o lib/features/iap/domain/entities/store_plan_entity.dart "$BASE/domain/entities/store_plan_entity.dart"
curl -o lib/features/iap/domain/enums/in_app_purchase_status.dart "$BASE/domain/enums/in_app_purchase_status.dart"
curl -o lib/features/iap/domain/repositories/in_app_purchase_repository.dart "$BASE/domain/repositories/in_app_purchase_repository.dart"
curl -o lib/features/iap/domain/usecases/iap_usecases.dart "$BASE/domain/usecases/iap_usecases.dart"
curl -o lib/features/iap/domain/usecases/verify_in_app_purchase_params.dart "$BASE/domain/usecases/verify_in_app_purchase_params.dart"

# Data
curl -o lib/features/iap/data/datasources/in_app_purchase_store_datasource.dart "$BASE/data/datasources/in_app_purchase_store_datasource.dart"
curl -o lib/features/iap/data/datasources/in_app_purchase_remote_datasource.dart "$BASE/data/datasources/in_app_purchase_remote_datasource.dart"
curl -o lib/features/iap/data/models/in_app_product_model.dart "$BASE/data/models/in_app_product_model.dart"
curl -o lib/features/iap/data/models/in_app_purchase_update_model.dart "$BASE/data/models/in_app_purchase_update_model.dart"
curl -o lib/features/iap/data/models/store_plan_model.dart "$BASE/data/models/store_plan_model.dart"
curl -o lib/features/iap/data/store_mapper/store_data_transformer.dart "$BASE/data/store_mapper/store_data_transformer.dart"
curl -o lib/features/iap/data/repositories/in_app_purchase_repository_impl.dart "$BASE/data/repositories/in_app_purchase_repository_impl.dart"

# Presentation
curl -o lib/features/iap/presentation/cubit/in_app_purchase_cubit.dart "$BASE/presentation/cubit/in_app_purchase_cubit.dart"
curl -o lib/features/iap/presentation/cubit/in_app_purchase_state.dart "$BASE/presentation/cubit/in_app_purchase_state.dart"

# Injection
curl -o lib/features/iap/iap_injection.dart "$BASE/iap_injection.dart"
```

**After download:** Read `flutter-kit-features/iap/HOW_TO_USE.md`

**Configure:**
1. Edit `constants.dart` → set your product IDs
2. Edit `verify_in_app_purchase_params.dart` → set your package name
3. Edit `iap_endpoints.dart` → set your API paths
4. Call `initIapInjection(onPurchaseSuccess: ...)` from `service_locator.dart`

---

### Auth + User Feature (Combined)
Login, signup, OTP, social login, logout + profile get/update, block/report/delete account.

**Requires:** flutter-kit-core + auth feature

```bash
BASE="https://raw.githubusercontent.com/YOUR_USERNAME/flutter-kit-features/main/auth_user/lib"

# Entities
curl -o lib/features/auth_user/domain/entities/user_entity.dart "$BASE/domain/entities/user_entity.dart"
curl -o lib/features/auth_user/domain/entities/location_data.dart "$BASE/domain/entities/location_data.dart"
curl -o lib/features/auth_user/domain/entities/check_username_response_entity.dart "$BASE/domain/entities/check_username_response_entity.dart"

# Repository + Use cases
curl -o lib/features/auth_user/domain/repositories/user_repository.dart "$BASE/domain/repositories/user_repository.dart"
curl -o lib/features/auth_user/domain/usecases/user_params.dart "$BASE/domain/usecases/user_params.dart"
curl -o lib/features/auth_user/domain/usecases/report_user_params.dart "$BASE/domain/usecases/report_user_params.dart"
curl -o lib/features/auth_user/domain/usecases/user_usecases.dart "$BASE/domain/usecases/user_usecases.dart"

# Data
curl -o lib/features/auth_user/data/datasources/user_endpoints.dart "$BASE/data/datasources/user_endpoints.dart"
curl -o lib/features/auth_user/data/datasources/user_remote_datasource.dart "$BASE/data/datasources/user_remote_datasource.dart"
curl -o lib/features/auth_user/data/models/user_model.dart "$BASE/data/models/user_model.dart"
curl -o lib/features/auth_user/data/models/check_username_response_model.dart "$BASE/data/models/check_username_response_model.dart"
curl -o lib/features/auth_user/data/repositories/user_repository_impl.dart "$BASE/data/repositories/user_repository_impl.dart"

# Presentation
curl -o lib/features/auth_user/presentation/cubit/user_cubit.dart "$BASE/presentation/cubit/user_cubit.dart"
curl -o lib/features/auth_user/presentation/cubit/user_state.dart "$BASE/presentation/cubit/user_state.dart"

# Injection
curl -o lib/features/auth_user/auth_user_injection.dart "$BASE/auth_user_injection.dart"
```

**After download:** Read `flutter-kit-features/auth_user/HOW_TO_USE.md`

**Configure:**
1. Edit `user_endpoints.dart` → set your API paths
2. Call `initAuthInjection()` + `initUserInjection()` from `service_locator.dart`

---

### ChatBot Feature
Generic AI chatbot with multi-turn sessions and local message caching.

**Requires:** flutter-kit-core (api_service, storage_service)

```bash
BASE="https://raw.githubusercontent.com/YOUR_USERNAME/flutter-kit-features/main/chatbot/lib"

curl -o lib/features/chatbot/domain/entities/chat_message_entity.dart "$BASE/domain/entities/chat_message_entity.dart"
curl -o lib/features/chatbot/domain/entities/chat_sender_type.dart "$BASE/domain/entities/chat_sender_type.dart"
curl -o lib/features/chatbot/domain/repositories/chatbot_repository.dart "$BASE/domain/repositories/chatbot_repository.dart"
curl -o lib/features/chatbot/domain/usecases/send_message_params.dart "$BASE/domain/usecases/send_message_params.dart"
curl -o lib/features/chatbot/domain/usecases/send_message_usecase.dart "$BASE/domain/usecases/send_message_usecase.dart"
curl -o lib/features/chatbot/data/datasources/chatbot_endpoints.dart "$BASE/data/datasources/chatbot_endpoints.dart"
curl -o lib/features/chatbot/data/datasources/chatbot_remote_datasource.dart "$BASE/data/datasources/chatbot_remote_datasource.dart"
curl -o lib/features/chatbot/data/models/chat_message_model.dart "$BASE/data/models/chat_message_model.dart"
curl -o lib/features/chatbot/data/repositories/chatbot_repository_impl.dart "$BASE/data/repositories/chatbot_repository_impl.dart"
curl -o lib/features/chatbot/presentation/bloc/chat_bot_bloc.dart "$BASE/presentation/bloc/chat_bot_bloc.dart"
curl -o lib/features/chatbot/presentation/bloc/chat_bot_event.dart "$BASE/presentation/bloc/chat_bot_event.dart"
curl -o lib/features/chatbot/presentation/bloc/chat_bot_state.dart "$BASE/presentation/bloc/chat_bot_state.dart"
curl -o lib/features/chatbot/chatbot_injection.dart "$BASE/chatbot_injection.dart"
```

**After download:** Read `flutter-kit-features/chatbot/HOW_TO_USE.md`

**Configure:**
1. Edit `chatbot_endpoints.dart` → set your endpoint path
2. Edit `chat_bot_bloc.dart` → set `initialPrompt`
3. Call `initChatBotInjection()` from `service_locator.dart`

---

### Remote Config Feature (Firebase)
Generic Firebase Remote Config wrapper with typed key access.

**Requires:** firebase_remote_config, firebase_core

```bash
BASE="https://raw.githubusercontent.com/YOUR_USERNAME/flutter-kit-features/main/remote_config/lib"

curl -o lib/features/remote_config/domain/entities/remote_config_entity.dart "$BASE/domain/entities/remote_config_entity.dart"
curl -o lib/features/remote_config/domain/repositories/remote_config_repository.dart "$BASE/domain/repositories/remote_config_repository.dart"
curl -o lib/features/remote_config/data/datasources/remote_config_datasource.dart "$BASE/data/datasources/remote_config_datasource.dart"
curl -o lib/features/remote_config/data/models/remote_config_model.dart "$BASE/data/models/remote_config_model.dart"
curl -o lib/features/remote_config/data/repositories/remote_config_repository_impl.dart "$BASE/data/repositories/remote_config_repository_impl.dart"
curl -o lib/features/remote_config/presentation/cubit/remote_config_cubit.dart "$BASE/presentation/cubit/remote_config_cubit.dart"
curl -o lib/features/remote_config/presentation/cubit/remote_config_state.dart "$BASE/presentation/cubit/remote_config_state.dart"
curl -o lib/features/remote_config/remote_config_injection.dart "$BASE/remote_config_injection.dart"
```

**After download:** Read `flutter-kit-features/remote_config/HOW_TO_USE.md`

**Configure:**
1. Add Firebase to your project (google-services.json / GoogleService-Info.plist)
2. Call `initRemoteConfigInjection()` from `service_locator.dart`
3. Call `sl<RemoteConfigCubit>().fetchConfig()` at app start
4. Create a `ConfigKeys` constants file for your key names
