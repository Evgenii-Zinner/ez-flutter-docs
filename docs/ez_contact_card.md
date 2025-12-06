# EzContactCard

A **customizable, composition-based** Flutter widget that creates contact-style cards with an avatar, headline, subtitle, and action area.

## Why use EzContactCard?

Building list items or contact cards often results in repetitive and messy code. The standard `ListTile` or custom `Row` compositions often suffer from:

*   **Duplicate Layouts:** You constantly rewrite the same `Row` -> `Avatar` -> `Column` -> `Text` hierarchy.
*   **Inconsistent Spacing:** Managing gaps between avatars, text, and action buttons often leads to inconsistent UI across different lists.
*   **Styling Boilerplate:** Applying the same decoration, padding, and text styles to multiple cards bloats your widget tree.
*   **Handling Overflows:** Forgetting to handle long names or subtitles often breaks layouts on smaller screens.

**EzContactCard** solves these problems with a single, production-ready widget:

*   **Standardized Layout:** Enforces a clean "Avatar - Content - Action" interaction pattern.
*   **Defensive Design:** Automatically handles text truncation, ensuring your UI *never* breaks with long names.
*   **Smart Defaults:** Comes with sensible padding, gap, and alignment defaults that look good out of the box.
*   **Total Control:** While opinionated about layout, it exposes **every styling property** (decoration, margin, text styles) so you can match your app's design system perfectly.

## API Reference

### Core Properties

| Property | Type | Description |
| :--- | :--- | :--- |
| `name` | `String` | **Required.** The main headline text. Automatically truncated with ellipsis if too long. |
| `subtitle` | `String?` | Optional secondary text displayed below the name. |
| `avatar` | `Widget` | **Required.** The widget to display on the left. Typically an `EzCircleAvatar`, but can be any widget. |
| `tail` | `Widget?` | Optional widget to display on the right edge (e.g., `IconButton`, checkmark). |

### Interaction

| Property | Type | Description |
| :--- | :--- | :--- |
| `onTap` | `VoidCallback?` | Callback for tap events. Triggers a splash effect. |
| `onLongPress` | `VoidCallback?` | Callback for long-press events. |
| `splashColor` | `Color?` | Custom color for the ink splash effect. |
| `highlightColor` | `Color?` | Custom color for the ink highlight effect. |

### Styling & Layout

| Property | Type | Description |
| :--- | :--- | :--- |
| `decoration` | `BoxDecoration?` | The decoration behind the card (background color, border, radius, shadow). |
| `margin` | `EdgeInsetsGeometry?` | Empty space surrounding the card. |
| `contentPadding` | `EdgeInsetsGeometry` | Padding inside the card. Defaults to `16.0` horizontal, `12.0` vertical. |
| `gap` | `double` | The space between the avatar, text column, and tail. Defaults to `16.0`. |
| `verticalAlignment` | `CrossAxisAlignment` | Vertical alignment of the row content. Defaults to `center`. |

### Text Styling

| Property | Type | Description |
| :--- | :--- | :--- |
| `nameStyle` | `TextStyle?` | Custom style for the name text. Defaults to `titleMedium`. |
| `subtitleStyle` | `TextStyle?` | Custom style for the subtitle text. Defaults to `bodyMedium` with `onSurfaceVariant` color. |
| `nameMaxLines` | `int` | Maximum lines for the name before truncation. Defaults to `1`. |
| `subtitleMaxLines` | `int` | Maximum lines for the subtitle before truncation. Defaults to `1`. |

## Usage Examples

### 1. Basic (Avatar & Name)

The simplest form. It automatically handles text truncation depending on available space.

```dart
EzContactCard(
  name: 'Jane Doe',
  avatar: EzCircleAvatar(name: 'Jane Doe'),
)
```

### 2. With Subtitle & Action

A common pattern for user lists (e.g., adding a "Call" button).

```dart
EzContactCard(
  name: 'John Smith',
  subtitle: 'Software Engineer',
  avatar: EzCircleAvatar(name: 'John Smith'),
  tail: IconButton(
    icon: Icon(Icons.phone),
    onPressed: () => _call(context),
  ),
  onTap: () => _viewProfile(context),
)
```

### 3. Fully Styled (Custom Design)

Match your specific design requirements while keeping the code clean.

```dart
EzContactCard(
  name: 'Admin User',
  subtitle: 'System Administrator',
  avatar: EzCircleAvatar(name: 'AU'),
  
  // Container Styling
  decoration: BoxDecoration(
    color: Colors.white,
    borderRadius: BorderRadius.circular(12),
    border: Border.all(color: Colors.grey.shade200),
    boxShadow: [
      BoxShadow(
        color: Colors.black.withOpacity(0.05),
        blurRadius: 4,
        offset: Offset(0, 2),
      ),
    ],
  ),
  margin: EdgeInsets.symmetric(vertical: 8, horizontal: 16),
  contentPadding: EdgeInsets.all(16),
  
  // Text Styling
  nameStyle: TextStyle(
    fontWeight: FontWeight.bold,
    fontSize: 16,
    color: Colors.indigo,
  ),
  subtitleStyle: TextStyle(
    color: Colors.indigo.shade300,
    fontStyle: FontStyle.italic,
  ),
  
  // Layout
  verticalAlignment: CrossAxisAlignment.start,
  gap: 20,
)
```

### 4. Custom Interaction Colors

Customize the ink response for better branding integration.

```dart
EzContactCard(
  name: 'Interactive Card',
  avatar: EzCircleAvatar(name: 'IC'),
  onTap: () {},
  splashColor: Colors.purple.withOpacity(0.2),
  highlightColor: Colors.purple.withOpacity(0.1),
)
```
