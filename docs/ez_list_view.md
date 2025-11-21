# EzListView

A defensive ListView.builder that prevents layout crashes from unbounded height/width constraints.

## Why use EzListView?

Flutter's `ListView` tries to expand to fill all available space in its scroll direction. When placed inside a parent with **unbounded constraints**, it breaks the layout entirely.

**Common crash scenarios:**

*   Placing a vertical list inside a `Column`
*   Placing a horizontal list inside a `Row`
*   Nesting it inside another `ListView`, `CustomScrollView`, or `SingleChildScrollView`
*   Using it inside a `Flex` or unconstrained `Card`

Instead of a simple error, this often breaks the build process entirely, causing the UI to vanish and spamming the console with cryptic messages like:

*   "Vertical viewport was given unbounded height."
*   "RenderBox was not laid out: RenderViewport... NEEDS-PAINT"
*   "Failed assertion: ... 'hasSize'"

**EzListView** is a defensive wrapper that detects these unbounded constraints *before* they cause damage:

*   **Auto-Detection:** Instantly identifies if it's in a `Column`, `Row`, or other unbounded parent.
*   **Crash Prevention:** Automatically applies a safe, bounded size (50% of screen) to ensure the widget renders instead of breaking.
*   **Developer Feedback (Debug Mode):** Displays a **red border** and logs a clear warning identifying the exact parent causing the issue.
*   **Silent Fix (Release Mode):** Applies the fix silently so your users never see a broken screen.

## API Reference

| Property | Type | Description |
| :--- | :--- | :--- |
| `itemBuilder` | `IndexedWidgetBuilder` | **Required.** Function that builds the widgets for each list item. |
| `itemCount` | `int?` | The number of items in the list. If null, the list is infinite. |
| `physics` | `ScrollPhysics?` | How the list should respond to user input (e.g., `BouncingScrollPhysics`). |
| `padding` | `EdgeInsetsGeometry?` | Padding around the list content. |

*See [ListView](https://api.flutter.dev/flutter/widgets/ListView-class.html) for additional inherited properties.*

## Usage Examples

### 1. Safe Inside Column (Crash Prevention)

This would normally crash, but `EzListView` handles it safely.

```dart
Column(
  children: [
    Text('My List'),
    
    // No crash! EzListView detects unbounded height and applies a fix.
    EzListView.builder(
      itemCount: 20,
      itemBuilder: (context, index) => ListTile(
        title: Text('Item $index'),
        leading: Icon(Icons.star),
      ),
    ),
  ],
)
```

### 2. The "Correct" Fix (Best Practice)

While `EzListView` prevents the crash, the best practice is to provide explicit constraints using `Expanded` or `SizedBox`.

```dart
Column(
  children: [
    Text('Header'),
    Expanded(
      child: EzListView.builder(
        itemCount: 50,
        itemBuilder: (context, index) => Card(
          child: ListTile(title: Text('Item $index')),
        ),
      ),
    ),
  ],
)
```

### 3. Standard Usage (No Unbounded Constraints)

When used normally (not inside a `Column`), it behaves exactly like `ListView.builder`.

```dart
EzListView.builder(
  itemCount: 100,
  padding: EdgeInsets.all(16),
  itemBuilder: (context, index) => Container(
    height: 60,
    margin: EdgeInsets.only(bottom: 8),
    color: Colors.blue,
    child: Center(child: Text('Item $index')),
  ),
)
```

### 4. Horizontal List Inside Row

`EzListView` also handles horizontal unbounded constraints (e.g., inside a `Row`).

```dart
Row(
  children: [
    Text('Label:'),
    EzListView.builder(
      scrollDirection: Axis.horizontal,
      itemCount: 10,
      itemBuilder: (context, index) => Container(
        width: 100,
        margin: EdgeInsets.all(4),
        color: Colors.green,
        child: Center(child: Text('$index')),
      ),
    ),
  ],
)
```
