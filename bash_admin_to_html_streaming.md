# Codex instructions for Bash-to-HTML console streaming

## Goal

Build and maintain a small Ubuntu administrator console where a browser page starts pre-approved backend actions and receives live Bash output in the HTML console.

The current pattern is:

```text
console.html
  ↓ EventSource / Server-Sent Events
admin-action.php
  ↓ proc_open()
approved Bash command or script
  ↓ stdout / stderr streamed line by line
console.html output window
```

Use this as a controlled admin console, not as a public terminal.

## Current file structure

Use these paths unless the project clearly uses another web root:

```text
/var/www/html/console.html
/var/www/html/admin-action.php
/usr/local/bin/test-admin-script.sh
/usr/local/bin/restart-apache-safe.sh
/etc/sudoers.d/ubuntu-admin-console
```

## Technology constraints

Use:

```text
Ubuntu
Apache
PHP 8.3
Plain JavaScript
Plain CSS
Server-Sent Events
Bash scripts
```

Avoid:

```text
jQuery
Raw command input fields
Direct browser-to-shell execution
PHP 8.4-specific syntax
Node.js unless explicitly requested
WebSockets unless two-way interaction is truly required
```

## Core design rule

The browser must never send a raw Linux command.

Correct:

```text
Browser sends: action=check_disk
PHP maps that to: df -h
```

Wrong:

```text
Browser sends: command=rm -rf /
PHP executes it
```

Every executable action must be listed in the PHP allowlist.

## Existing frontend behavior

`console.html` should:

1. Show a modern admin dashboard.
2. Display buttons for approved actions.
3. Open an EventSource stream to `/admin-action.php?action=ACTION_NAME`.
4. Append each streamed line to the console output.
5. Auto-scroll to the newest output.
6. Disable action buttons while an action is running.
7. Allow clearing, copying and stopping the visible stream.
8. Ask confirmation before dangerous actions.
9. Never provide a free text command input.

The important frontend call is:

```javascript
currentStream = new EventSource('/admin-action.php?action=' + encodeURIComponent(actionName));
```

Each action button should use:

```html
<button class="action-button" data-action="check_disk">
  Check disk usage
</button>
```

For dangerous actions, add a confirmation message:

```html
<button class="action-button danger" data-action="restart_apache" data-confirm="Restart Apache now?">
  Restart Apache
</button>
```

## Existing backend behavior

`admin-action.php` should:

1. Send `text/event-stream` headers.
2. Disable buffering where possible.
3. Read the requested `action`.
4. Check the action against the allowlist.
5. Run the matching command with `proc_open()`.
6. Read stdout and stderr line by line.
7. Send stdout as normal SSE messages.
8. Send stderr as named `error_message` SSE events.
9. Send a final `--- Done, exit code: X ---` message.
10. Never run unknown actions.

The key PHP headers are:

```php
header('Content-Type: text/event-stream; charset=utf-8');
header('Cache-Control: no-cache, no-transform');
header('Connection: keep-alive');
header('X-Accel-Buffering: no');
```

The key PHP execution method is `proc_open()`, because it allows reading stdout and stderr through pipes.

## Backend allowlist pattern

Add actions only in the `$allowedActions` array.

Example:

```php
$allowedActions = [
    'check_disk' => [
        'label' => 'Check disk usage',
        'command' => 'df -h',
    ],
    'check_memory' => [
        'label' => 'Check memory usage',
        'command' => 'free -h',
    ],
    'run_test_script' => [
        'label' => 'Run test Bash script',
        'command' => 'bash /usr/local/bin/test-admin-script.sh',
    ],
];
```

Never build commands directly from `$_GET`, `$_POST`, cookies, headers or user input.

## How to add a new safe action

### Step 1: Create a Bash script

Create a script in `/usr/local/bin`.

Example:

```bash
sudo nano /usr/local/bin/check-drupal-status.sh
```

Script:

```bash
#!/bin/bash

# Stop when a serious command fails.
set -e

# Show what this script does.
echo "Checking Drupal status..."

# Go to the Drupal project root.
cd /var/www/html

# Show the current directory so the admin knows where the command runs.
echo "Working directory: $(pwd)"

# Check Drupal status through Drush.
vendor/bin/drush status

echo "Drupal status check finished."
```

Make it executable:

```bash
sudo chmod +x /usr/local/bin/check-drupal-status.sh
```

Test it as the web server user:

```bash
sudo -u www-data bash /usr/local/bin/check-drupal-status.sh
```

### Step 2: Add the backend action

Edit:

```text
/var/www/html/admin-action.php
```

Add this to `$allowedActions`:

```php
'check_drupal_status' => [
    'label' => 'Check Drupal status',
    'command' => 'bash /usr/local/bin/check-drupal-status.sh',
],
```

### Step 3: Add the frontend button

Edit:

```text
/var/www/html/console.html
```

Add:

```html
<button class="action-button secondary" data-action="check_drupal_status">
  Check Drupal status
</button>
```

### Step 4: Test in browser

Open:

```text
http://YOUR-SERVER-IP/console.html
```

Click:

```text
Check Drupal status
```

Expected result:

```text
Starting action: check_drupal_status
Action accepted: Check Drupal status
Checking Drupal status...
...
--- Done, exit code: 0 ---
```

## How to add an action that needs root

Do not run large inline sudo commands from PHP.

Create a small root-safe script instead.

Example:

```bash
sudo nano /usr/local/bin/restart-apache-safe.sh
```

Script:

```bash
#!/bin/bash

# Stop if a command fails.
set -e

# Explain what is happening.
echo "Restarting Apache..."

# Restart Apache.
systemctl restart apache2

# Show whether Apache is active.
echo "Checking Apache status..."
systemctl is-active apache2

echo "Apache restart completed."
```

Make it executable:

```bash
sudo chmod +x /usr/local/bin/restart-apache-safe.sh
```

Allow only this script in sudoers:

```bash
echo 'www-data ALL=(root) NOPASSWD: /usr/local/bin/restart-apache-safe.sh' | sudo tee /etc/sudoers.d/ubuntu-admin-console
sudo chmod 440 /etc/sudoers.d/ubuntu-admin-console
```

Add backend action:

```php
'restart_apache' => [
    'label' => 'Restart Apache',
    'command' => 'sudo /usr/local/bin/restart-apache-safe.sh',
],
```

Add frontend button:

```html
<button class="action-button danger" data-action="restart_apache" data-confirm="Restart Apache now?">
  Restart Apache
</button>
```

## Output streaming requirements

Bash scripts should use `echo` frequently.

Good:

```bash
echo "Starting backup..."
sleep 1
echo "Creating archive..."
sleep 1
echo "Backup complete."
```

Bad:

```bash
# Runs silently for 10 minutes.
tar -czf backup.tar.gz /var/www/html
```

For long commands, add progress messages before and after each major step.

PHP should run commands with line buffering:

```php
$streamingCommand = 'stdbuf -oL -eL ' . $command;
```

This helps avoid output appearing only after the script finishes.

## Troubleshooting

### Output only appears after the script finishes

Check:

```text
Is stdbuf used?
Does the Bash script use echo?
Is Apache or Nginx buffering the response?
Is X-Accel-Buffering: no present?
Is PHP output buffering disabled?
```

Useful PHP pattern:

```php
while (ob_get_level() > 0) {
    ob_end_flush();
}

ob_implicit_flush(true);
```

### Browser shows stream connection closed or failed

Test the backend directly:

```bash
curl http://localhost/admin-action.php?action=run_test_script
```

Check Apache errors:

```bash
sudo tail -f /var/log/apache2/error.log
```

Check if PHP has permission to execute the script:

```bash
sudo -u www-data bash /usr/local/bin/test-admin-script.sh
```

### Sudo command fails

Check the sudoers file:

```bash
sudo cat /etc/sudoers.d/ubuntu-admin-console
```

Check the exact command path in PHP.

This must match the sudoers command exactly:

```php
'command' => 'sudo /usr/local/bin/restart-apache-safe.sh',
```

### Drush command fails

Check the Drupal root path:

```bash
cd /var/www/html
vendor/bin/drush status
```

If Drupal lives somewhere else, update the PHP command or Bash script path.

## Security requirements

Implement these before using this beyond a private test server:

```text
Protect console.html with login
Protect admin-action.php with login
Use HTTPS
Restrict access by IP where possible
Use action allowlists only
Log every action
Add confirmation for destructive actions
Avoid broad sudo rules
Keep scripts small and auditable
Do not expose the console publicly
```

Recommended extra log file:

```text
/var/log/ubuntu-admin-console/actions.log
```

Example logging line in PHP:

```php
error_log(date('c') . ' action=' . $action . ' user=' . get_current_user() . PHP_EOL, 3, '/var/log/ubuntu-admin-console/actions.log');
```

Create the log directory:

```bash
sudo mkdir -p /var/log/ubuntu-admin-console
sudo chown www-data:www-data /var/log/ubuntu-admin-console
sudo chmod 750 /var/log/ubuntu-admin-console
```

## Recommended action groups

Start with read-only actions:

```text
check_disk
check_memory
check_load
check_apache_status
check_php_version
check_drupal_status
check_database_status
view_recent_apache_errors
view_recent_drupal_logs
```

Then add controlled maintenance actions:

```text
drupal_cache_rebuild
restart_apache
restart_php_fpm
run_backup
check_backup_status
pull_git_status
composer_validate
```

Only add destructive actions when authentication and logging are in place.

## Suggested future expansion

### Server selector

For DS03 and VPS01, add a server selector in the frontend.

Do not let the browser send arbitrary hostnames.

Correct backend structure:

```php
$allowedServers = [
    'ds03' => [
        'label' => 'DS03',
        'ssh_target' => 'admin@ds03',
    ],
    'vps01' => [
        'label' => 'VPS01',
        'ssh_target' => 'admin@vps01',
    ],
];
```

Then map approved action + approved server.

### Action history

Store finished actions with:

```text
timestamp
server
action
exit code
duration
user
```

### Cancel real process

The current stop button closes the browser stream. It does not necessarily kill the backend process.

To implement real cancel support:

1. Store process IDs.
2. Assign every run a unique job ID.
3. Add `/cancel-action.php?job_id=...`.
4. Kill only processes owned by that job.
5. Log cancellation.

Do not implement cancel by killing broad process names.

### Drupal 11 module version

Later this can become a Drupal 11.2.5 custom module.

Suggested module name:

```text
ubuntu_admin_console
```

Suggested routes:

```text
/admin/config/system/ubuntu-admin-console
/admin/config/system/ubuntu-admin-console/stream/{action}
```

Suggested permissions:

```text
view ubuntu admin console
run ubuntu admin read only actions
run ubuntu admin maintenance actions
run ubuntu admin restart actions
```

Use Drupal permissions instead of a standalone public HTML page.

## Acceptance checklist for Codex

Codex should consider the task complete only when:

```text
console.html opens in the browser
Buttons trigger backend actions
Output streams line by line
Unknown actions are blocked
stderr appears as errors
Final exit code is shown
Dangerous actions require confirmation
No raw command input exists
Bash scripts are executable
www-data can run allowed scripts
sudo is limited to exact safe scripts only
```

## Main files to modify

Usually modify only:

```text
/var/www/html/console.html
/var/www/html/admin-action.php
/usr/local/bin/*.sh
```

Avoid changing Apache, sudoers or system service configuration unless explicitly requested.

## Notes for Codex

When expanding this project:

1. Preserve the allowlist-first security model.
2. Keep code simple and heavily commented.
3. Use PHP 8.3-compatible syntax.
4. Use plain JavaScript.
5. Use plain CSS.
6. Avoid jQuery.
7. Add new functionality as small, testable actions.
8. Prefer Bash scripts over large inline shell strings.
9. Test as `www-data`, not only as root.
10. Keep dangerous operations isolated behind exact sudoers entries.
