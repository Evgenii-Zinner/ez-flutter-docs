# EzCircleAvatar

A smart avatar widget that automatically generates initials and contrasting background colors from names.

## Why use EzCircleAvatar?

Displaying user avatars typically involves repetitive boilerplate code. The standard `CircleAvatar` doesn't handle:

*   **Inconsistent Colors:** Generating background colors from names requires custom logic that often differs between screens, creating a jarring experience where "John" might appear red on one screen and blue on another.
*   **Initials Extraction:** Manually extracting "JD" from "John Doe" (or handling single names, empty strings, etc.) becomes tedious and error-prone.
*   **Contrast Issues:** Ensuring text remains readable against generated backgrounds is frequently overlooked.

**EzCircleAvatar** solves these problems automatically:

*   **Consistent Hashing:** Uses a deterministic hash to generate the *same background color everywhere* in your app. "John" will always be the same shade, ensuring UI consistency.
*   **Smart Initials:** Automatically extracts initials from any name format (e.g., "John Doe" → "JD", "Alice" → "A").
*   **Automatic Contrast:** Intelligently selects black or white foreground text based on background luminance for perfect readability.
*   **Defensive Design:** Gracefully handles empty names, null images, and loading errors with sensible fallbacks.

## API Reference

| Property | Type | Description |
| :--- | :--- | :--- |
| `name` | `String` | **Required.** The name used to generate initials and a deterministic background color via hashing. |
| `backgroundColor` | `Color?` | Override the auto-generated background color. If null, a color is generated from `name`. |
| `foregroundColor` | `Color?` | Override the auto-selected text color. If null, automatically chooses black or white based on background luminance. |
| `backgroundImage` | `ImageProvider?` | The background image. If loading fails, falls back to displaying initials. |
| `foregroundImage` | `ImageProvider?` | The foreground image (overlays the background). Typically used for badges or status indicators. |
| `child` | `Widget?` | Custom widget to display inside the circle. Overrides auto-generated initials. |
| `radius` | `double?` | The size of the avatar. Defaults to 20 logical pixels. |
| `minRadius` | `double?` | The minimum size constraint for the avatar. |
| `maxRadius` | `double?` | The maximum size constraint for the avatar. |
| `onBackgroundImageError` | `ImageErrorListener?` | Callback invoked when the background image fails to load. |
| `onForegroundImageError` | `ImageErrorListener?` | Callback invoked when the foreground image fails to load. |

*See [CircleAvatar](https://api.flutter.dev/flutter/material/CircleAvatar-class.html) for additional inherited properties.*

## Usage Examples

### 1. Basic (Auto-Color & Initials)

The simplest use case. The widget automatically generates consistent colors and extracts initials.

```dart
EzCircleAvatar(name: 'Jane Doe')  // Shows "JD"
EzCircleAvatar(name: 'Alice')      // Shows "A"
```

### 2. With Image (Graceful Fallback)

If the image fails to load, it automatically falls back to initials.

```dart
EzCircleAvatar(
  name: 'John Smith',
  backgroundImage: NetworkImage('https://example.com/avatar.jpg'),
  radius: 30,
)
```

### 3. Custom Styling

Override the auto-generated colors and size.

```dart
EzCircleAvatar(
  name: 'Admin User',
  backgroundColor: Colors.black,
  foregroundColor: Colors.amber,
  radius: 40,
)
```

### 4. Edge Cases

The widget handles empty names and special characters gracefully.

```dart
EzCircleAvatar(name: '')      // Shows default person icon
EzCircleAvatar(name: '   ')   // Shows default person icon (whitespace)
EzCircleAvatar(name: '@#!')   // Shows "@"
```
