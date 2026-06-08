# Init and Install Scripts Agent

## Scope

Activates for shell scripts that install, initialize, bootstrap, verify, or
update machine-side components inside this repository, especially:

- `/.scripts/.vpscontainer/*.sh`
- `/.scripts/.ddev/*.sh`
- `/.scripts/*.sh` automation that orchestrates system setup
- `init_*.sh` and `install_*.sh` scripts that are part of the setup flow

It does **not** apply to Drupal PHP code, Twig, JavaScript, README content, or
general repository documentation. Those tasks must use their own more specific
agents.

## Rules

1. **Shell contract**
   - Use `#!/usr/bin/env bash`.
   - Use `set -euo pipefail`.
   - Source `source_bm_colors.sh` for user-facing script output.
   - Source `resolve_dir_paths.sh` when filesystem paths are needed.

2. **Messaging**
   - Print a script banner with `MainTopicLine`.
   - Print the current task with `TopicLine`.
   - Use `Blue` for progress and action lines.
   - Use `OkLine`, `WarningLine`, and `ErrorLine` for success, recoverable,
     and hard-failure states.
   - Use `InputLine` for all operator prompts.
   - Avoid bare `echo` for human-facing narration.

3. **Status and run modes**
   - Support `--status` for every install/init script that can report state.
   - Keep status mode read-only.
   - Show the current state before asking the operator whether to keep or
     replace existing state.
   - Use `--run` or an explicit run flag for scripts that can either report or
     mutate state.

4. **Install flow design**
   - Keep installation work in `install_*.sh` scripts.
   - Keep orchestration and sequencing in `init_*.sh` scripts.
   - Allow a higher-level init script to bootstrap a prerequisite installer when a required dependency is missing, but keep the prerequisite installer itself in an `install_*.sh` file.
   - Prefer helper scripts over duplicated inline logic.
   - Use the repository’s verification helpers when the script already relies
     on them.
   - Do not assume hard-coded instance paths; derive paths from the shared path
     resolver or from the current instance root.

5. **Operator interaction**
   - Prompt only when the operator must choose between keeping or replacing an
     existing installation or configuration.
   - Default to a safe no-op when a prompt is unanswered.
   - If the script supports verification-only behavior, run that after a refused
     install/update so the current state is still reported.

6. **Repository hygiene**
   - Keep automation inside `.scripts/`.
   - Leave nested repositories and their own automation isolated from the
     parent repository.
   - If a script needs Drupal context, use the established project bootstrap
     path rather than inventing a new one.

## Notes

This agent replaces the previous script-communication placeholder. Its purpose
is to make init/install shell behavior explicit as a first-class Codex scope,
separate from repository rules about layout or general documentation.
