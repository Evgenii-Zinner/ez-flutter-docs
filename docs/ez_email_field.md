# EZEmailField

A pre-validated, highly customizable email text field with built-in regex and error handling.

## Why use EZEmailField?

Implementing email fields repeatedly involves tedious boilerplate. The standard `TextFormField` requires you to:

*   **Copy-Paste Regex:** Manually add the same email validation regex to every form.
*   **Write Validation Logic:** Repeat the same `if (value.isEmpty) ... else if (!regex.hasMatch) ...` pattern.
*   **Inconsistent UX:** Different parts of your app might accept different email formats or show different error messages.

**EZEmailField** encapsulates best practices into a single widget:

*   **Built-in Validation:** Comes with a robust, industry-standard regex for email validation out of the box.
*   **Defensive Defaults:** Handles empty states and formatting errors automatically with clear messages.
*   **Unified UX:** Ensures email input behaves consistently across your entire application.
*   **Zero-Config:** Just drop it in a `Form`, and it works immediately.

## API Reference

| Property | Type | Description |
| :--- | :--- | :--- |
| `labelText` | `String` | The label displayed above the field. Defaults to `'Email'`. |
| `hintText` | `String` | Placeholder text shown when the field is empty. Defaults to `'Enter your email address'`. |
| `required` | `bool` | If true, validation ensures the field is not empty. Defaults to `true`. |
| `controller` | `TextEditingController?` | External controller to manage the field's content. If null, an internal controller is used. |
| `decoration` | `InputDecoration?` | Custom decoration to override default styling (border, icons, padding, etc.). |
| `customValidator` | `FormFieldValidator<String>?` | Custom validation function. If provided, it *replaces* the default email validation entirely. |
| `emailRegex` | `RegExp?` | Custom regex for email validation. If provided, replaces the default complex regex. |
| `onChanged` | `ValueChanged<String>?` | Callback invoked when the text changes. |
| `readOnly` | `bool` | If true, the field cannot be edited. Defaults to `false`. |
| `autofocus` | `bool` | If true, the field automatically receives focus when displayed. Defaults to `false`. |
| `obscureText` | `bool` | If true, hides the text (typically for passwords). Defaults to `false`. |
| `maxLines` | `int?` | Maximum number of lines. Defaults to `1`. |
| `minLines` | `int?` | Minimum number of lines. |
| `style` | `TextStyle?` | Text style for the input. Defaults to 16px. |
| `textAlign` | `TextAlign` | Text alignment. Defaults to `TextAlign.start`. |
| `textInputAction` | `TextInputAction?` | Action button on the keyboard. Defaults to `TextInputAction.next`. |
| `keyboardType` | `TextInputType?` | Keyboard type. Defaults to `TextInputType.emailAddress`. |
| `onFieldSubmitted` | `ValueChanged<String>?` | Callback when the user submits the field (e.g., presses "done"). |
| `onEditingComplete` | `VoidCallback?` | Callback when editing is complete. |

*See [TextFormField](https://api.flutter.dev/flutter/material/TextFormField-class.html) for additional inherited properties.*

## Usage Examples

### 1. Basic (Zero Config)

Just drop it in a `Form`. It automatically validates email format.

```dart
Form(
  key: _formKey,
  child: Column(
    children: [
      EZEmailField(),
      ElevatedButton(
        onPressed: () {
          if (_formKey.currentState!.validate()) {
            // Email is valid
          }
        },
        child: Text('Submit'),
      ),
    ],
  ),
)
```

### 2. Custom Styling

Override labels, hints, and decoration.

```dart
EZEmailField(
  labelText: 'Work Email',
  hintText: 'john.doe@company.com',
  decoration: InputDecoration(
    border: OutlineInputBorder(),
    prefixIcon: Icon(Icons.work_outline),
    filled: true,
  ),
)
```

### 3. Custom Validation (Domain Restriction)

Add extra validation on top of the built-in email check.

```dart
EZEmailField(
  customValidator: (value) {
    if (value != null && !value.endsWith('@company.com')) {
      return 'Corporate email required';
    }
    return null; // Passes custom validation
  },
  decoration: InputDecoration(
    labelText: 'Corporate Email',
    helperText: 'Must end with @company.com',
  ),
)
```

### 4. With Controller

Use an external controller to access or manipulate the text.

```dart
final _emailController = TextEditingController();

EZEmailField(
  controller: _emailController,
  onChanged: (value) {
    print('Email changed: $value');
  },
)
```
