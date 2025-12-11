# Repository Guidelines

## Project Structure & Module Organization
Each directory named `flipbook-{A|B|C}-{LT|LV|CZ}` is a self-contained plugin containing `flipbook.php`, an `app/` MVC stack (controller, model, helpers), rendered assets in `data/` (`files/`, `javascript/`, `style/`), locale imagery under `images/`, and third-party notices in `Licensing/`. Keep edits scoped to the correct locale-level folder; never share data or styles between variants because WordPress loads only the selected plugin directory. Duplicate any cross-locale helper into `app/functions.php` to prevent regressions.

## Build, Test, and Development Commands
- `find flipbook-A-LT -name '*.php' -print0 | xargs -0 php -l` — lint the PHP sources before committing (swap in the locale you touched).
- `php -S 127.0.0.1:8000 -t flipbook-A-LV/data` — serves the generated HTML package for quick asset debugging.
- `ln -s $PWD/flipbook-B-CZ ~/sites/wp/wp-content/plugins/flipbook && wp plugin activate flipbook` — link into a WordPress install to view `/wp-admin/admin.php?page=flipbook_show_books`.
Reset the symlink whenever you switch between plugin variants so WordPress points at the folder under test.

## Coding Style & Naming Conventions
Use tabs, brace-on-newline, and WordPress-friendly naming: `StudlyCaps` classes, `snake_case` hooks, and BEM-ish CSS already present in `data/style`. Escape output, wrap strings with `__`/`_e`, and centralize reusable logic under `app/functions.php`. When importing vendor CSS/JS, copy it into the plugin directory instead of editing upstream files.

## Testing Guidelines
Because no automated suite exists, rely on linting plus manual verification. Every update must render in both the admin “View Book” screen and the `[flipbook id="X"]` shortcode. When assets change, open `data/index.html` in desktop and tablet breakpoints and confirm the console stays clean. Document browser/locale coverage and repro steps in your PR description.

## Commit & Pull Request Guidelines
Commits follow `<locale>: <action>` (e.g., `lv: fix cover links`). Keep messages lower-case and scoped to a single locale-level directory. Pull requests should link to the tracking task, list the touched plugin folders, outline manual test steps, and attach screenshots or recordings of the admin listing plus the embedded shortcode. Call out migrations (new PDFs, resized assets) and request review from another locale owner when modifying shared PHP in `app/`.

## Security & Configuration Tips
Do not commit WordPress secrets or environment files; store credentials outside the repo. When testing locally, ensure the symlink targets only directories within this repository to avoid exposing other plugins in version control. Keep dependencies vendored under each plugin’s `data/` tree so releases remain self-contained.
