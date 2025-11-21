---
description: Official documentation for EZ Flutter Widgets.
keywords: flutter, widgets, ez_flutter, mobile development
---

# EZ Flutter Widgets

A collection of **simple, defensive, and developer-friendly** Flutter widgets designed to prevent common crashes and reduce boilerplate.

## Available Widgets

<div class="grid cards" markdown>

-   :material-account-circle:{ .lg .middle } **[EzCircleAvatar](ez_circle_avatar.md)**

    ---

    A smart avatar widget that automatically generates initials and contrasting background colors from names. Handles image loading errors gracefully.

-   :material-email-check:{ .lg .middle } **[EZEmailField](ez_email_field.md)**

    ---

    A pre-validated, highly customizable email text field with built-in regex and error handling. Just drop it in a `Form` and go.

-   :material-view-grid:{ .lg .middle } **[EzGridView](ez_grid_view.md)**

    ---

    A crash-safe `GridView.builder` that automatically handles unbounded constraints in Columns and Rows, preventing "Vertical viewport was given unbounded height" errors.

-   :material-view-list:{ .lg .middle } **[EzListView](ez_list_view.md)**

    ---

    A defensive `ListView.builder` that prevents layout crashes from unbounded height/width constraints. It acts as a drop-in replacement for standard ListViews.

-   :material-view-day:{ .lg .middle } **[EzCustomScrollView](ez_custom_scroll_view.md)**

    ---

    A defensive wrapper for `CustomScrollView` that safely handles unbounded constraints with helpful debug info.

-   :material-arrow-expand-vertical:{ .lg .middle } **[EzExpanded](ez_expanded.md)**

    ---

    A defensive widget that provides flex-based expansion behavior while safely handling unbounded constraints. It prevents "Incorrect use of ParentDataWidget" crashes.

</div>

## Installation

Add the desired package to your `pubspec.yaml`:

```yaml
dependencies:
  ez_circle_avatar: ^0.1.0
  ez_email_field: ^0.1.0
  ez_grid_view: ^0.1.0
  ez_list_view: ^0.1.0
  ez_custom_scroll_view: ^0.1.0
  ez_expanded: ^0.1.0
```

## Philosophy

1.  **Defensive:** Widgets should never crash your app. They should handle edge cases (unbounded constraints, nulls, errors) gracefully.
2.  **Developer-Friendly:** Clear error messages in debug mode help you fix issues fast, while silent fixes in release mode keep users happy.
3.  **Simple:** Minimal configuration required. Sensible defaults for everything.

## Support

If you find these widgets helpful, please consider supporting the project:

*   [GitHub Sponsors](https://github.com/sponsors/Evgenii-Zinner)
*   [Buy Me a Coffee](https://buymeacoffee.com/evgeniizinner)
*   [Thanks.dev](https://thanks.dev/u/gh/evgenii-zinner)
