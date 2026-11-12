# ReachInbox CLI — public releases

Install and run the unofficial ReachInbox account-management CLI without cloning
a repository or reviewing source code. The source Git repository stays private;
the installable package includes readable runtime code. Node.js **22+** and npm
are required. Works on Linux, macOS, Windows and Git Bash.

## Install

```bash
npm install --global https://github.com/SirDannyMunn/reachinbox-cli-releases/releases/download/v0.3.0/reachinbox-local-cli-0.3.0.tgz
reachinbox --help
```

No GitHub account or npm registry login is needed to download this public package.
If global installation needs permissions, use a user-managed Node installation,
a writable npm prefix, or the no-global-install command below.

## Run without a global install

```bash
npx --yes --package=https://github.com/SirDannyMunn/reachinbox-cli-releases/releases/download/v0.3.0/reachinbox-local-cli-0.3.0.tgz reachinbox --help
```

## Instructions to give an agent

> Install ReachInbox CLI from this repository's versioned public release using
> the npm install command above. Verify with `reachinbox --help`. It requires
> Node.js 22+. A source checkout is unnecessary. Run `reachinbox login` to sign in
> and save a local session, or obtain a session token separately. Never put secrets in a command argument,
> source file, or report. Commands that change settings preview by default;
> use `--apply` only for the settings changes I request.

## Authentication

Run `reachinbox login` and enter your ReachInbox email/password at the prompts.
The password is hidden and never saved. The returned token is verified and saved
locally, then reused automatically. `reachinbox auth status` shows local session
identity/expiry without exposing the token; `reachinbox logout` deletes it.

For agents, pipe a password from a secret manager:
`secret-manager-command | reachinbox login --email you@example.com --password-stdin`.
No password flag is accepted. Git Bash users whose terminal lacks Node TTY access
can use stdin or perform interactive login in PowerShell.

The local token is plaintext, restricted to the current user: Linux/macOS use
`~/.config/reachinbox-cli/session.json` (or XDG_CONFIG_HOME), with file mode 600;
Windows uses `%APPDATA%\reachinbox-cli\session.json` with a current-user ACL.
Do not share or commit it. Logout is local only and does not revoke other sessions.

Email/password login is supported. OAuth-only login and browser security or
verification challenges require the browser. No automatic refresh or challenge
bypass is provided. Re-run login on expiry. Existing `REACHINBOX_TOKEN`,
`--token-stdin`, and `--token-file` options override the saved session.

```bash
reachinbox login
reachinbox auth status
reachinbox warmup industries
reachinbox accounts list --all
reachinbox warmup get --id 1234567
reachinbox accounts overview --id 1234567
reachinbox warmup sent --id 1234567 --format csv --out sent.csv
reachinbox warmup health --id 1234567 --start 2026-10-30 --end 2026-11-06 --format csv --out health.csv
```

The CLI covers warm-up configuration, all 53 industry choices, activity and
health reports, CSV downloads, account signatures and campaign settings,
tracking domains, DNS, tags and SMTP/IMAP management. Run `reachinbox --help`,
`reachinbox warmup fields`, `reachinbox settings fields`, `reachinbox smtp fields`,
and `reachinbox operations list` for available commands and fields.

## Releases and verification

Use a version-pinned URL for reproducible agent installations. Each release
contains the npm-compatible `.tgz` package and `SHA256SUMS`. This distribution
does not require npm lifecycle scripts; installs can include `--ignore-scripts`.
No credentials or captured account data are bundled. This is an unofficial tool
using web-app APIs, which can change independently of these releases.

To remove a global installation: `npm uninstall --global reachinbox-local-cli`.

## Campaigns and lead-list imports (v0.3)

```bash
reachinbox campaigns list --all
reachinbox campaigns create --name "Founder outreach" --apply
reachinbox lists create --name "UK founders" --apply
# Replace 5501 with the returned list ID. Preview before applying.
reachinbox leads import --list-id 5501 --file prospects.csv
reachinbox leads import --list-id 5501 --file prospects.csv --apply --journal import-uk.json
reachinbox lists leads --id 5501 --all
```

CSV needs an email column; extra columns become custom fields. JSON arrays also work.
Use --map mapping.json for source-header-to-field mappings. All rows validate
before upload; duplicate emails within a file are skipped by default. Imports
use batches and a progress journal, with no automatic retries after uncertain
writes. Accepted means queued for background processing, not completed. List
imports explicitly disable credit-consuming verification.

Commands include campaign sequences, schedules, options, sending-account
selection, explicit lifecycle actions, lead-list management and campaign lead
updates/actions. Every mutation previews unless --apply. Creation makes a draft;
start is a separate explicit command. Importing contacts into a running campaign
can cause sending. Run reachinbox --help and management operations for details.
The installed README and examples include the full payload guide.

The campaign/list contracts were mapped from signed-in web-app client scripts.
33 local tests pass; live campaign mutations have not been exercised.
