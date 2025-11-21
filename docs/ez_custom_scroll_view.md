# EzCustomScrollView

A defensive wrapper for CustomScrollView that safely handles unbounded constraints with helpful debug info.

## Why use EzCustomScrollView?

Flutter's `CustomScrollView` is a powerful widget for building complex scrollable layouts with slivers. However, when placed inside a parent with **unbounded constraints**, it breaks the layout entirely.

**Common crash scenarios:**

*   Placing a vertical scroll view inside a `Column`
*   Placing a horizontal scroll view inside a `Row`
*   Nesting it inside another `ListView`, `CustomScrollView`, or `SingleChildScrollView`
*   Using it inside a `Flex` or unconstrained `Card`

Instead of a simple error, this often breaks the build process entirely, causing the UI to vanish and spamming the console with cryptic messages like:

*   "Vertical viewport was given unbounded height."
*   "RenderBox was not laid out: RenderViewport... NEEDS-PAINT"
*   "Failed assertion: ... 'hasSize'"

**EzCustomScrollView** is a defensive wrapper that detects these unbounded constraints *before* they cause damage:

*   **Auto-Detection:** Instantly identifies if it's in a `Column`, `Row`, or other unbounded parent.
*   **Crash Prevention:** Automatically applies a safe, bounded size (50% of screen) to ensure the widget renders instead of breaking.
*   **Developer Feedback (Debug Mode):** Displays a **red border** and logs a clear warning identifying the exact parent causing the issue.
*   **Silent Fix (Release Mode):** Applies the fix silently so your users never see a broken screen.

## API Reference

| Property | Type | Description |
| :--- | :--- | :--- |
| `slivers` | `List<Widget>` | **Required.** The list of slivers to place inside the scroll view (e.g., `SliverList`, `SliverGrid`, `SliverAppBar`). |
| `scrollDirection` | `Axis` | The axis along which the scroll view scrolls. Defaults to `Axis.vertical`. |
| `reverse` | `bool` | Whether the scroll view scrolls in the reading direction. Defaults to `false`. |
| `controller` | `ScrollController?` | An object that can be used to control the position to which this scroll view is scrolled. |
| `primary` | `bool?` | Whether this is the primary scroll view associated with the parent `PrimaryScrollController`. |
| `physics` | `ScrollPhysics?` | How the scroll view should respond to user input. |
| `shrinkWrap` | `bool` | Whether the extent of the scroll view in the `scrollDirection` should be determined by the contents being viewed. Defaults to `false`. |
| `center` | `Key?` | The key of the sliver that should be centered in the viewport. |
| `anchor` | `double` | The relative position of the zero scroll offset. Defaults to `0.0`. |
| `cacheExtent` | `double?` | The viewport will cache children that are up to this many pixels away from the visible area. |
| `semanticChildCount` | `int?` | The number of children that will contribute semantic information. |
| `dragStartBehavior` | `DragStartBehavior` | Determines the way that drag start behavior is handled. Defaults to `DragStartBehavior.start`. |
| `keyboardDismissBehavior` | `ScrollViewKeyboardDismissBehavior` | Defines how the keyboard dismisses when scrolling. Defaults to `ScrollViewKeyboardDismissBehavior.manual`. |
| `restorationId` | `String?` | Restoration ID to save and restore the scroll offset. |
| `clipBehavior` | `Clip` | The content will be clipped (or not) according to this option. Defaults to `Clip.hardEdge`. |

*See [CustomScrollView](https://api.flutter.dev/flutter/widgets/CustomScrollView-class.html) for additional inherited properties.*

## Usage Examples

### 1. Safe Inside Column (Crash Prevention)

This would normally crash, but `EzCustomScrollView` handles it safely.

```dart
Column(
  children: [
    Text('Header'),
    
    // No crash! EzCustomScrollView detects unbounded height.
    EzCustomScrollView(
      slivers: [
        SliverAppBar(
          title: Text('Safe Sliver'),
          floating: true,
        ),
        SliverList(
          delegate: SliverChildBuilderDelegate(
            (context, index) => ListTile(title: Text('Item $index')),
            childCount: 20,
          ),
        ),
      ],
    ),
  ],
)
```

### 2. The "Correct" Fix (Best Practice)

While `EzCustomScrollView` prevents the crash, the best practice is to provide explicit constraints using `Expanded` or `SizedBox`.

```dart
Column(
  children: [
    Text('Header'),
    Expanded(
      child: EzCustomScrollView(
        slivers: [
          SliverGrid(
            gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
              crossAxisCount: 2,
            ),
            delegate: SliverChildBuilderDelegate(
              (context, index) => Card(child: Text('$index')),
              childCount: 50,
            ),
          ),
        ],
      ),
    ),
  ],
)
```

### 3. Complex Layout with Multiple Slivers

Combine multiple sliver types for advanced scrolling effects.

```dart
EzCustomScrollView(
  slivers: [
    SliverAppBar(
      expandedHeight: 200,
      flexibleSpace: FlexibleSpaceBar(
        title: Text('My App'),
        background: Image.network('https://picsum.photos/400/200', fit: BoxFit.cover),
      ),
    ),
    SliverToBoxAdapter(
      child: Padding(
        padding: EdgeInsets.all(16),
        child: Text('Section 1', style: TextStyle(fontSize: 24)),
      ),
    ),
    SliverList(
      delegate: SliverChildBuilderDelegate(
        (context, index) => ListTile(title: Text('Item $index')),
        childCount: 10,
      ),
    ),
    SliverGrid(
      gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(crossAxisCount: 3),
      delegate: SliverChildBuilderDelegate(
        (context, index) => Container(color: Colors.blue, child: Center(child: Text('$index'))),
        childCount: 9,
      ),
    ),
  ],
)
```

### 4. Horizontal Scroll Inside Row

`EzCustomScrollView` also handles horizontal unbounded constraints.

```dart
Row(
  children: [
    Text('Label:'),
    EzCustomScrollView(
      scrollDirection: Axis.horizontal,
      slivers: [
        SliverList(
          delegate: SliverChildBuilderDelegate(
            (context, index) => Container(
              width: 100,
              margin: EdgeInsets.all(4),
              color: Colors.green,
              child: Center(child: Text('$index')),
            ),
            childCount: 10,
          ),
        ),
      ],
    ),
  ],
)
```
