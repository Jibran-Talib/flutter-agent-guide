# TextField Validation Skill

## Validators (core/utils/validators.dart)

All validators return `String?` — `null` means valid, a string is the error message.

```dart
class Validators {
  static String? validateEmail(String? value) {
    if (value == null || value.isEmpty) return 'Email is required';
    final emailRegex = RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$');
    if (!emailRegex.hasMatch(value)) return 'Enter a valid email address';
    return null;
  }

  static String? validatePassword(String? value) {
    if (value == null || value.isEmpty) return 'Password is required';
    if (value.length < 8) return 'Password must be at least 8 characters';
    return null;
  }

  static String? validateConfirmPassword(String? value, String password) {
    if (value == null || value.isEmpty) return 'Please confirm your password';
    if (value != password) return 'Passwords do not match';
    return null;
  }

  static String? validatePhone(String? value) {
    if (value == null || value.isEmpty) return 'Phone number is required';
    final phoneRegex = RegExp(r'^\+?[1-9]\d{9,14}$');
    if (!phoneRegex.hasMatch(value)) return 'Enter a valid phone number';
    return null;
  }

  static String? validateUsername(String? value) {
    if (value == null || value.isEmpty) return 'Username is required';
    if (value.length < 3) return 'Username must be at least 3 characters';
    return null;
  }

  static String? validateOtp(String? value) {
    if (value == null || value.isEmpty) return 'OTP is required';
    if (value.length != 6) return 'OTP must be 6 digits';
    return null;
  }

  static String? validateRequired(String? value, [String? fieldName]) {
    if (value == null || value.trim().isEmpty) {
      return '${fieldName ?? 'This field'} is required';
    }
    return null;
  }
}
```

---

## Form Setup

Always use `GlobalKey<FormState>` and `AutovalidateMode.onUserInteraction`:

```dart
class LoginScreen extends StatefulWidget { ... }

class _LoginScreenState extends State<LoginScreen> {
  final _formKey = GlobalKey<FormState>();
  final _emailController = TextEditingController();
  final _passwordController = TextEditingController();

  @override
  void dispose() {
    _emailController.dispose();
    _passwordController.dispose();
    super.dispose();
  }

  void _submit() {
    if (_formKey.currentState!.validate()) {
      context.read<AuthCubit>().login(LoginRequestParams(
        email: _emailController.text.trim(),
        password: _passwordController.text,
      ));
    }
  }

  @override
  Widget build(BuildContext context) {
    return Form(
      key: _formKey,
      autovalidateMode: AutovalidateMode.onUserInteraction,
      child: Column(
        children: [
          InputField(
            labelText: 'Email',
            controller: _emailController,
            validator: Validators.validateEmail,
          ),
          InputField(
            labelText: 'Password',
            controller: _passwordController,
            obscureText: true,
            validator: Validators.validatePassword,
          ),
          ButtonComponent.primary(text: 'Login', onPressed: _submit),
        ],
      ),
    );
  }
}
```

---

## Confirm Password Validator

Needs access to the other controller, so pass it as a closure:

```dart
InputField(
  labelText: 'Confirm Password',
  controller: _confirmController,
  obscureText: true,
  validator: (value) => Validators.validateConfirmPassword(
    value,
    _passwordController.text,
  ),
)
```

---

## Custom Field Validators

For project-specific fields, follow the same `String? Function(String?)` signature:

```dart
String? validateBio(String? value) {
  if (value == null || value.isEmpty) return 'Bio is required';
  if (value.length > 150) return 'Bio must be under 150 characters';
  return null;
}
```
