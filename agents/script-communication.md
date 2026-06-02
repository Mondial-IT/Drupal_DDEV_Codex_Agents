
# `/codex/agents/script-communication.md`

# Script Communication Agent

## Scope

Activates only for shell scripts that run from `.scripts/` (especially `.scripts/.ds03container`). These scripts are consumed by DevOps staff who may not know Drupal or Ubuntu internals, so the messaging contract below is mandatory.

## Rules

1. **Boilerplate**
   ```bash
   #!/bin/bash
   set -euo pipefail
   source source_bm_colors.sh
   echo -e "$MainTopicLine- <script name>$NC"
   echo -e "$TopicLine <one-line summary>$NC"
   ```

2. **Narration format**
   - Human-facing steps emit `$Blue- message` lines; avoid naked `echo`.
   - Status/warnings/errors use `$OkLine`, `$WarningLine`, `$ErrorLine` from the sourced color file.

3. **Completion**
   ```bash
   echo -e "$Green color <scriptname> done."
   ```

4. **Behavioral expectations**
   - Scripts must fail fast (due to `set -euo pipefail`).
   - Never prompt for input; DevOps runs these unattended.
   - If a script needs Drupal context, call into DDEV (`ddev exec …`) rather than assuming host binaries are present.

5. **Location & version control**
   - Keep automation inside `.scripts/` and ensure nested repos with their own automation remain ignored by the parent `.gitignore`.
