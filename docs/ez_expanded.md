# EzExpanded

A defensive widget that provides flex-based expansion behavior while safely handling unbounded constraints.

## Why use EzExpanded?

Developers often want widgets to "fill available space" in layouts. Flutter's `Expanded` widget provides this for `Row` and `Column`, but it has strict requirements:

*   **Must be inside Flex:** Using `Expanded` outside of `Row`, `Column`, or `Flex` causes a crash: "Incorrect use of ParentDataWidget."
*   **Unbounded Constraints:** Even when used correctly inside Flex, if the Flex itself has unbounded constraints, the layout can still break.

**EzExpanded** follows the same defensive pattern as other EZ widgets:

*   **Auto-Detection:** Uses `LayoutBuilder` to detect unbounded constraints (infinite width/height).
*   **Crash Prevention:** Automatically applies a safe, bounded size (50% of screen) when unbounded.
*   **Developer Feedback (Debug Mode):** Displays a **red border** and logs a clear warning identifying the parent.
*   **Silent Fix (Release Mode):** Applies the fix silently so users never see a broken screen.
*   **Flex Behavior:** When constraints are bounded, uses `Flexible` with `FlexFit.tight` to provide the same expansion as `Expanded`.

## API Reference

| Property | Type | Description |
| :--- | :--- | :--- |
| `child` | `Widget` | **Required.** The widget below this widget in the tree. |
| `flex` | `int` | The flex factor to use for this child. Defaults to `1`. Determines how much space the child occupies relative to other flex children. |

*See [Expanded](https://api.flutter.dev/flutter/widgets/Expanded-class.html) and [Flexible](https://api.flutter.dev/flutter/widgets/Flexible-class.html) for more details on flex behavior.*

## Usage Examples

### 1. Safe with Unbounded Constraints

This would normally crash or cause layout errors, but `EzExpanded` handles it safely.

```dart
Column(
  children: [
    Text('Header'),
    EzExpanded(
      child: Container(
        color: Colors.blue,
        child: Center(child: Text('Fills Space')),
      ),
    ),
    Text('Footer'),
  ],
)
```

### 2. Correct Usage (Bounded Constraints)

When used in a properly constrained context, it behaves like `Expanded`.

```dart
SizedBox(
  height: 400,
  child: Column(
    children: [
      Text('Header'),
      EzExpanded(
        child: Container(
          color: Colors.green,
          child: Center(child: Text('Expands to Fill')),
        ),
      ),
    ],
  ),
)
```

### 3. With Flex Factor

Control the expansion ratio just like `Expanded`.

```dart
Row(
  children: [
    EzExpanded(
      flex: 2,
      child: Container(color: Colors.red),
    ),
    EzExpanded(
      flex: 1,
      child: Container(color: Colors.blue),
    ),
  ],
)
```

### 4. Horizontal Unbounded (Safe)

Also handles unbounded width constraints.

```dart
Row(
  children: [
    Text('Label:'),
    EzExpanded(
      child: Container(
        color: Colors.purple,
        child: Center(child: Text('Safe Width')),
      ),
    ),
  ],
)
```
