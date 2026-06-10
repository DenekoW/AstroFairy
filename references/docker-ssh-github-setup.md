# Docker SSH GitHub Setup (this environment)

Last verified: 2026-06-09

## Environment

- Docker container running Hermes Agent
- `$HOME` is `/opt/data/profiles/fairy/home`
- SSH looks for keys in `/opt/data/.ssh/` (NOT `$HOME/.ssh/`)
- No `sudo`, no root access

## SSH Key Setup

```bash
# Generate key
ssh-keygen -t ed25519 -C "astrofairy@hermes" -f ~/.ssh/id_ed25519 -N ""

# Copy to SSH's actual home
cp ~/.ssh/id_ed25519 /opt/data/.ssh/
cp ~/.ssh/id_ed25519.pub /opt/data/.ssh/
chmod 600 /opt/data/.ssh/id_ed25519

# Add GitHub host key
ssh-keyscan -H github.com >> /opt/data/.ssh/known_hosts

# Test
ssh -T git@github.com
```

## Git Remote Setup

```bash
git remote set-url origin git@github.com:USER/REPO.git
git push -u origin main
```

## Pitfalls

- SSH debug log says `identity file /opt/data/.ssh/id_ed25519 type -1` even though `~/.ssh/id_ed25519` exists — SSH uses a different home than `$HOME`. Copy key to `/opt/data/.ssh/`.
- `ssh-keyscan` without `-H` may not write to the right known_hosts path. Use `-H`.
- The first `ssh` connection will warn about new host — add `-o StrictHostKeyChecking=accept-new` to suppress.
- `agent-browser` cannot be installed (Node.js v20, need v24; no root for npm global). Browser-based tools are unavailable.
- Playwright Chromium download (~150MB) times out. Use curl-based approaches only.
