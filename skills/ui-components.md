# UI Components Skill

## Available Widgets (from flutter-kit-widgets)

Download only what you need. See `catalogs/widgets.md` for curl commands.

---

## ButtonComponent

```dart
// Primary gradient button
ButtonComponent.primary(
  text: 'Login',
  onPressed: () => _login(),
  isLoading: state.isLoadingOf(AuthAction.login),
)

// Grey (secondary) button
ButtonComponent.grey(text: 'Cancel', onPressed: () => context.pop())

// Delete (red) button
ButtonComponent.delete(text: 'Delete Account', onPressed: () => _delete())

// Outline button
ButtonComponent.outline(text: 'Skip', onPressed: () => context.go(Routes.home.path))

// Edit button (icon + text)
ButtonComponent.edit(text: 'Edit Profile', onPressed: () => _edit())

// Disabled state
ButtonComponent.disabled(text: 'Processing...')
```

---

## InputField

```dart
InputField(
  labelText: 'Email Address',
  hintText: 'you@email.com',
  controller: _emailController,
  keyboardType: TextInputType.emailAddress,
  validator: Validators.validateEmail,
  // Optional:
  obscureText: false,
  readOnly: false,
  maxLines: 1,
  textInputAction: TextInputAction.next,
  onFieldSubmitted: (_) => FocusScope.of(context).nextFocus(),
)

// Password field (shows eye toggle automatically when obscureText: true)
InputField(
  labelText: 'Password',
  controller: _passwordController,
  obscureText: true,
  validator: Validators.validatePassword,
)
```

---

## ResponseWidget

Show inline after form submission — success or error feedback:

```dart
// In BlocBuilder
if (state.status == AuthStatus.failure)
  ResponseWidget.error(title: state.message ?? 'Login failed'),
if (state.status == AuthStatus.success)
  ResponseWidget.success(title: 'Logged in successfully!'),
```

---

## AppLoader / LoaderOverlay

```dart
// Full-screen overlay
Stack(
  children: [
    _buildContent(),
    if (state.isLoading) const LoaderOverlay(),
  ],
)

// Inline spinner
const AppLoader()
```

---

## CustomSnackBar

```dart
// Success
CustomSnackBar.success(context, 'Profile updated successfully!')

// Error
CustomSnackBar.error(context, state.message ?? 'Something went wrong')
```

---

## AppDialog

```dart
// Logout confirmation
AppDialog.logout(onConfirm: () => sl<SessionCubit>().logout())

// Delete confirmation
AppDialog.delete(
  title: 'Delete Post',
  onConfirm: () => context.read<PostCubit>().deletePost(id),
)

// Generic success
AppDialog.success(title: 'Done!', message: 'Your profile was updated.')

// Generic warning
AppDialog.warning(title: 'Warning', message: 'This cannot be undone.')

// Block user
AppDialog.blockUser(onConfirm: () => _block(userId))

// Leave group
AppDialog.leave(onConfirm: () => _leaveGroup())

// Subscription prompt (navigate to paywall)
AppDialog.subscription(onConfirm: () => context.push(Routes.subscription.path))
```

---

## CachedImageComponent

```dart
CachedImageComponent(
  imageUrl: user['avatar'] ?? '',
  width: 60,
  height: 60,
  borderRadius: BorderRadius.circular(30),
)
```

---

## TextComponent

```dart
// Regular
TextComponent(
  text: 'Hello World',
  fontsize: 18,
  fontWeight: FontWeight.bold,
  color: context.colors.onBackground,
)

// Gradient text
TextComponent(
  text: 'Premium',
  fontsize: 24,
  fontWeight: FontWeight.w800,
  color: AppNavigator.gradients.primaryGradient,  // List<Color>
)

// With character limit
TextComponent(
  text: longBio,
  maxCharacters: 100,
  fontsize: 14,
)
```

---

## Color Access Patterns

```dart
// In a widget build method (requires BuildContext)
context.colors.primary
context.colors.onBackground
context.colors.surface

// Static, no context needed (from AppNavigator)
AppNavigator.colors.primary
AppNavigator.gradients.primaryGradient  // List<Color>

// Hard-coded palette fallback
AppColors.primary
AppColors.error
```
