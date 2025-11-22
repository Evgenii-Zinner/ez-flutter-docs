# EzPasswordField

A secure, developer-friendly password field with built-in visibility toggle, smart validation, and robustness features.

## Why use EzPasswordField?

Implementing password fields typically involves repetitive boilerplate:

*   **Manual Toggling:** You have to manually manage `obscureText` state and add an `IconButton` for visibility every time.
*   **Complex Validation:** Writing regex for "at least 8 characters, one uppercase, one digit..." is tedious and error-prone.
*   **Poor UX:** Showing errors one by one (e.g., "Too short" -> fix -> "Need uppercase" -> fix -> "Need digit") frustrates users.
*   **Inconsistent Rules:** Different parts of your app might enforce slightly different password policies.

**EzPasswordField** solves these problems:

*   **Built-in Visibility:** Comes with a working eye icon out of the box. No state management required.
*   **Smart Validation:** Accumulates all missing requirements into a single, clear message (e.g., *"Issues: no spaces, at least 8 characters, a digit"*).
*   **Robustness Flags:** Easily prohibit specific characters like spaces or dashes with simple boolean flags.
*   **Zero Config:** Defaults to strong security practices (min length 8, uppercase, lowercase, digits, special chars).

## API Reference

| Property | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `controller` | `TextEditingController?` | `null` | Controls the text being edited. |
| `labelText` | `String?` | `'Password'` | Optional text that describes the input field. |
| `hintText` | `String?` | `null` | Text that suggests what sort of input the field accepts. |
| `minLength` | `int` | `8` | Minimum required length for the password. |
| `requireUppercase` | `bool` | `true` | Whether to require at least one uppercase letter. |
| `requireLowercase` | `bool` | `true` | Whether to require at least one lowercase letter. |
| `requireDigits` | `bool` | `true` | Whether to require at least one digit. |
| `requireSpecialChars` | `bool` | `true` | Whether to require at least one special character. |
| `prohibitSpaces` | `bool` | `true` | Whether to prohibit spaces. |
| `prohibitDashes` | `bool` | `false` | Whether to prohibit dashes. |
| `prohibitLetters` | `bool` | `false` | Whether to prohibit letters. |
| `prohibitDigits` | `bool` | `false` | Whether to prohibit digits. |
| `prohibitSpecialChars` | `bool` | `false` | Whether to prohibit special characters. |
| `validator` | `FormFieldValidator<String>?` | `null` | Additional validation logic. Merged with built-in validation. |
| `decoration` | `InputDecoration?` | `null` | The decoration to show around the text field. |
| `onChanged` | `ValueChanged<String>?` | `null` | Called when the user initiates a change to the TextField's value. |

## Usage Examples

### 1. Basic Usage

The simplest use case. Just drop it in a `Form`, and you get a secure password field with visibility toggle and strong validation.

```dart
EzPasswordField(
  controller: _passwordController,
)
```

### 2. PIN Mode (Digits Only)

Restrict input to digits only for PINs. Use `prohibitLetters` and `prohibitSpecialChars` to enforce the format.

```dart
EzPasswordField(
  labelText: 'PIN',
  hintText: 'Enter 4-6 digits',
  minLength: 4,
  requireUppercase: false,
  requireLowercase: false,
  requireSpecialChars: false,
  requireDigits: true,
  // Robustness flags
  prohibitLetters: true,
  prohibitSpecialChars: true,
  prohibitSpaces: true,
  prohibitDashes: true,
)
```

### 3. Robustness (No Spaces)

Prevent users from entering spaces or dashes in their passwords.

```dart
EzPasswordField(
  labelText: 'Secure Password',
  prohibitSpaces: true,
  prohibitDashes: true,
)
```

### 4. Custom Validation

You can add your own validation logic on top of the built-in rules.

```dart
EzPasswordField(
  validator: (value) {
    if (value != null && value.toLowerCase().contains('password')) {
      return 'Password cannot contain "password"';
    }
    return null;
  },
)
```

### 5. Custom Styling

Style it just like a standard `TextFormField`.

```dart
EzPasswordField(
  decoration: InputDecoration(
    border: OutlineInputBorder(),
    prefixIcon: Icon(Icons.lock),
    filled: true,
    fillColor: Colors.grey[200],
  ),
)
```
