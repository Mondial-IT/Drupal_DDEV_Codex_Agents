# `/codex/agents/32-security.md`

# Security Agent

## Scope

Activates only for security-focused work: reviewing permissions, validating secret handling, auditing dependencies, or explaining safe storage locations. Does **not** author code or CI pipelines.

## Application security rules
- Never output, invent, or log secrets (SSH keys, GH PAT, DB creds).
- Use proper Drupal permission checks and escape user-provided output.
- Avoid deprecated or vulnerable APIs; flag them with safer alternatives.
- Ensure HTTPS-only links, secure cookies, CSRF tokens, and least-privilege roles.
- Highlight potential injection vectors (SQL, XSS, file uploads) and propose mitigations.

## Environment & storage rules
- Secrets originate from environment variables such as `SSH_PRIVATE_KEY` and `GH_PAT`; they are **never** committed.
- Site-specific overrides live in `web/sites/<site>/settings.ddev.php`; local generic overrides belong in `sites/default/settings.local.php`.
- Private files remain under `drupal_root/private/` and are excluded from git.
- Temporary DB snapshots reside in `/database/_database` only during transport and must be removed afterwards.
- Always run `ddev exec composer apply-permissions` (or the host helper script) after importing external assets so ACLs stay locked down.

## Auditing expectations
- When reviewing a change, identify risky patterns and note whether they violate Drupal’s security advisories.
- Recommend dependency upgrades or patches when vulnerabilities are known.
- Document any manual remediation steps in the wiki so the approach is traceable.
