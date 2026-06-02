# process and rules to create patches

To create a patch for Drupal files:

1. Only work inside `drupal_root/.patches`. Never edit files directly under `drupal_root/web` when preparing a patch.
2. Recreate the path starting from `web` inside `.patches`, so `.patches/web/...` mirrors the target file’s path. A copy of `.patches/web` back to `drupal_root/web` should overwrite the intended file and nothing else.
3. Inside that mirrored directory, ensure an `unpatched` subdirectory exists.
4. Copy the original file into `unpatched/` and append `.unpatched` to its filename (e.g., `filename.php.unpatched`). Do not modify this backup.
5. Copy the original file again into the mirrored path (outside `unpatched`) to serve as the working copy.
6. Apply your fixes only to the working copy in `.patches/web/...` (never outside `.patches`).
7. Add a `{filename}.md` alongside the patched file describing the issue and the solution applied.
8. Add a brief code comment in the patched file noting the patch (include “Mondial-IT BV” and the current date) explaining why the change was needed.
