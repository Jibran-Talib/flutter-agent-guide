# Widget Catalog — flutter-kit-widgets

Each widget is individually downloadable. Only download what you need.

**Base URL:** `https://raw.githubusercontent.com/YOUR_USERNAME/flutter-kit-widgets/main`

> TODO: Replace `YOUR_USERNAME` with the actual GitHub username after pushing the repo.

---

## Available Widgets

### ButtonComponent
Gradient button with factories: primary, grey, delete, outline, edit.

```bash
curl -o lib/shared/widgets/button_component.dart \
  "https://raw.githubusercontent.com/YOUR_USERNAME/flutter-kit-widgets/main/lib/shared/widgets/button_component.dart"
```

**Requires:** `flutter_screenutil`, `flutter_animate`, `iconly`, `AppNavigator` (from core)

**Usage:**
```dart
ButtonComponent.primary(text: 'Submit', onPressed: () => _submit())
ButtonComponent.delete(text: 'Delete', onPressed: () => _delete())
```

---

### InputField
Full-featured text field with label, validation, password toggle, keyboard types.

```bash
curl -o lib/shared/widgets/text_field_component.dart \
  "https://raw.githubusercontent.com/YOUR_USERNAME/flutter-kit-widgets/main/lib/shared/widgets/text_field_component.dart"
```

**Requires:** `theme_extensions.dart` (from core)

**Usage:**
```dart
InputField(
  labelText: 'Email',
  controller: _emailController,
  keyboardType: TextInputType.emailAddress,
  validator: Validators.validateEmail,
)
```

---

### ResponseWidget
Inline success/error feedback banner.

```bash
curl -o lib/shared/widgets/response_widget.dart \
  "https://raw.githubusercontent.com/YOUR_USERNAME/flutter-kit-widgets/main/lib/shared/widgets/response_widget.dart"
```

**Usage:**
```dart
ResponseWidget.success(title: 'Saved successfully')
ResponseWidget.error(title: state.message ?? 'Something went wrong')
```

---

### AppLoader
Gradient shimmer loading spinner.

```bash
curl -o lib/shared/widgets/app_loader.dart \
  "https://raw.githubusercontent.com/YOUR_USERNAME/flutter-kit-widgets/main/lib/shared/widgets/app_loader.dart"
```

**Requires:** `material_loading_indicator`, `AppNavigator` (from core)

---

### LoaderOverlay
Full-screen black overlay with centered AppLoader.

```bash
curl -o lib/shared/widgets/loader_overlay.dart \
  "https://raw.githubusercontent.com/YOUR_USERNAME/flutter-kit-widgets/main/lib/shared/widgets/loader_overlay.dart"
```

**Usage:**
```dart
Stack(children: [
  _mainContent(),
  if (state.isLoading) const LoaderOverlay(),
])
```

---

### CustomSnackBar
Overlay-based top-sliding snackbar with auto-dismiss.

```bash
curl -o lib/shared/widgets/custom_snackbar.dart \
  "https://raw.githubusercontent.com/YOUR_USERNAME/flutter-kit-widgets/main/lib/shared/widgets/custom_snackbar.dart"
```

**Usage:**
```dart
CustomSnackBar.success(context, 'Profile updated!')
CustomSnackBar.error(context, state.message ?? 'Error occurred')
```

---

### AppDialog
Static dialog utility — delete, warning, success, logout, subscription, report, etc.

```bash
# Download both files
curl -o lib/shared/widgets/dialogs_widget/app_dialog.dart \
  "https://raw.githubusercontent.com/YOUR_USERNAME/flutter-kit-widgets/main/lib/shared/widgets/dialogs_widget/app_dialog.dart"

curl -o lib/shared/widgets/dialogs_widget/dialog_types.dart \
  "https://raw.githubusercontent.com/YOUR_USERNAME/flutter-kit-widgets/main/lib/shared/widgets/dialogs_widget/dialog_types.dart"
```

**Requires:** `AppNavigator`, `AppRouter.navigatorKey` (from core), `flutter_screenutil`, `flutter_animate`, `iconly`

**Usage:**
```dart
AppDialog.logout(onConfirm: () => sl<SessionCubit>().logout())
AppDialog.delete(title: 'Delete Post', onConfirm: () => _delete())
AppDialog.success(title: 'Done!', message: 'Your changes were saved.')
```

---

### CachedImageComponent
CachedNetworkImage wrapper with loading placeholder.

```bash
curl -o lib/shared/widgets/cache_image_component.dart \
  "https://raw.githubusercontent.com/YOUR_USERNAME/flutter-kit-widgets/main/lib/shared/widgets/cache_image_component.dart"
```

**Requires:** `cached_network_image`

---

### TextComponent
Text widget with gradient text support and character limit.

```bash
curl -o lib/shared/widgets/text_component.dart \
  "https://raw.githubusercontent.com/YOUR_USERNAME/flutter-kit-widgets/main/lib/shared/widgets/text_component.dart"
```

**Usage:**
```dart
TextComponent(text: 'Hello', fontsize: 18, fontWeight: FontWeight.bold)
// Gradient text:
TextComponent(text: 'Premium', color: AppNavigator.gradients.primaryGradient)
```

---

## Download All Widgets

```bash
BASE="https://raw.githubusercontent.com/YOUR_USERNAME/flutter-kit-widgets/main"

curl -o lib/shared/widgets/button_component.dart "$BASE/lib/shared/widgets/button_component.dart"
curl -o lib/shared/widgets/text_field_component.dart "$BASE/lib/shared/widgets/text_field_component.dart"
curl -o lib/shared/widgets/response_widget.dart "$BASE/lib/shared/widgets/response_widget.dart"
curl -o lib/shared/widgets/app_loader.dart "$BASE/lib/shared/widgets/app_loader.dart"
curl -o lib/shared/widgets/loader_overlay.dart "$BASE/lib/shared/widgets/loader_overlay.dart"
curl -o lib/shared/widgets/custom_snackbar.dart "$BASE/lib/shared/widgets/custom_snackbar.dart"
curl -o lib/shared/widgets/cache_image_component.dart "$BASE/lib/shared/widgets/cache_image_component.dart"
curl -o lib/shared/widgets/text_component.dart "$BASE/lib/shared/widgets/text_component.dart"
curl -o lib/shared/widgets/dialogs_widget/app_dialog.dart "$BASE/lib/shared/widgets/dialogs_widget/app_dialog.dart"
curl -o lib/shared/widgets/dialogs_widget/dialog_types.dart "$BASE/lib/shared/widgets/dialogs_widget/dialog_types.dart"
```
