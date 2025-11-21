# EzGridView

A crash-safe GridView.builder that automatically handles unbounded constraints in Columns and Rows.

## Why use EzGridView?

Flutter's `GridView` tries to expand to fill all available space in its scroll direction. When placed inside a parent with **unbounded constraints**, it breaks the layout entirely.

**Common crash scenarios:**

*   Placing a vertical grid inside a `Column`
*   Placing a horizontal grid inside a `Row`
*   Nesting it inside another `ListView`, `CustomScrollView`, or `SingleChildScrollView`
*   Using it inside a `Flex` or unconstrained `Card`

Instead of a simple error, this often breaks the build process entirely, causing the UI to vanish and spamming the console with cryptic messages like:

*   "Vertical viewport was given unbounded height."
*   "RenderBox was not laid out: RenderViewport... NEEDS-PAINT"
*   "Failed assertion: ... 'hasSize'"

**EzGridView** is a defensive wrapper that detects these unbounded constraints *before* they cause damage:

*   **Auto-Detection:** Instantly identifies if it's in a `Column`, `Row`, or other unbounded parent.
*   **Crash Prevention:** Automatically applies a safe, bounded size (50% of screen) to ensure the widget renders instead of breaking.
*   **Developer Feedback (Debug Mode):** Displays a **red border** and logs a clear warning identifying the exact parent causing the issue.
*   **Silent Fix (Release Mode):** Applies the fix silently so your users never see a broken screen.

## API Reference

| Property | Type | Description |
| :--- | :--- | :--- |
| `gridDelegate` | `SliverGridDelegate` | **Required.** Controls the layout of tiles (e.g., `SliverGridDelegateWithFixedCrossAxisCount`). |
| `itemBuilder` | `IndexedWidgetBuilder` | **Required.** Function that builds the widgets for each grid item. |
| `itemCount` | `int?` | The number of items in the grid. If null, the grid is infinite. |
| `physics` | `ScrollPhysics?` | How the grid should respond to user input (e.g., `BouncingScrollPhysics`). |
| `padding` | `EdgeInsetsGeometry?` | Padding around the grid content. |

*See [GridView](https://api.flutter.dev/flutter/widgets/GridView-class.html) for additional inherited properties.*

## Usage Examples

### 1. Safe Inside Column (Crash Prevention)

This would normally crash, but `EzGridView` handles it safely.

```dart
Column(
  children: [
    Text('My Gallery'),
    
    // No crash! EzGridView detects unbounded height and applies a fix.
    EzGridView.builder(
      gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
        crossAxisCount: 3,
        mainAxisSpacing: 8,
        crossAxisSpacing: 8,
      ),
      itemCount: 9,
      itemBuilder: (context, index) => Container(
        color: Colors.blue,
        child: Center(child: Text('$index')),
      ),
    ),
  ],
)
```

### 2. The "Correct" Fix (Best Practice)

While `EzGridView` prevents the crash, the best practice is to provide explicit constraints using `Expanded` or `SizedBox`.

```dart
Column(
  children: [
    Text('Header'),
    Expanded(
      child: EzGridView.builder(
        gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
          crossAxisCount: 2,
        ),
        itemCount: 20,
        itemBuilder: (context, index) => Card(
          child: Center(child: Text('Item $index')),
        ),
      ),
    ),
  ],
)
```

### 3. Standard Usage (No Unbounded Constraints)

When used normally (not inside a `Column`), it behaves exactly like `GridView.builder`.

```dart
EzGridView.builder(
  gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
    crossAxisCount: 4,
  ),
  itemCount: 100,
  itemBuilder: (context, index) => Image.network('https://picsum.photos/200'),
)
```
