# EZ Flutter Widgets Documentation

Official documentation for [EZ Flutter Widgets](https://flutter.ezinner.com/), a collection of defensive and developer-friendly Flutter widgets.

## Overview

This repository contains the source code for the EZ Flutter Widgets documentation website, built with [MkDocs](https://www.mkdocs.org/) and [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

## Features

-   **Defensive Widgets:** Documentation for crash-safe widgets like `EzExpanded`, `EzGridView`, and `EzListView`.
-   **Modern Design:** Uses Material for MkDocs with a custom card-based index.
-   **Searchable:** Instant search for all documentation content.

## Running Locally

1.  **Install Python:** Ensure you have Python installed.
2.  **Install Dependencies:**
    ```bash
    pip install mkdocs-material
    ```
3.  **Serve the Site:**
    ```bash
    mkdocs serve
    ```
    Open `http://127.0.0.1:8000/` in your browser.

## Deployment

This site is designed to be deployed to GitHub Pages.

```bash
mkdocs gh-deploy
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
