# ReachInbox CLI — public releases

Install and run the unofficial ReachInbox account-management CLI without cloning
a repository or reviewing source code. The source Git repository stays private;
the installable package includes readable runtime code. Node.js **22+** and npm
are required. Works on Linux, macOS, Windows and Git Bash.

## Install

```bash
npm install --global https://github.com/SirDannyMunn/reachinbox-cli-releases/releases/download/v0.1.0/reachinbox-local-cli-0.1.0.tgz
reachinbox --help
```

No GitHub account or npm registry login is needed to download this public package.
If global installation needs permissions, use a user-managed Node installation,
a writable npm prefix, or the no-global-install command below.

## Run without a global install

```bash
npx --yes --package=https://github.com/SirDannyMunn/reachinbox-cli-releases/releases/download/v0.1.0/reachinbox-local-cli-0.1.0.tgz reachinbox --help
```

## Instructions to give an agent

> Install ReachInbox CLI from this repository's versioned public release using
> the npm install command above. Verify with `reachinbox --help`. It requires
> Node.js 22+. A source checkout is unnecessary. Obtain my ReachInbox session
> token separately before accessing my account; never put it in a command argument,
> source file, or report. Commands that change settings preview by default;
> use `--apply` only for the settings changes I request.

## Authentication

Provide your existing ReachInbox session bearer token through `REACHINBOX_TOKEN`,
`--token-stdin`, or `--token-file /path/to/private-token`. Installation needs no
ReachInbox credentials. Protect a token file with `chmod 600` on Linux. Session
expiry requires signing in again and refreshing the token.

```bash
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
