# TokenRouter Switch

TokenRouter Switch is a desktop utility for signing in to TokenRouter, viewing available API keys, and applying a selected key to local AI coding agents.

The app is built with Electron, Vite, React, and TypeScript. It supports macOS and Windows packaging, and uses GitHub Releases as the public update channel.

## Features

- TokenRouter account sign-in with email/password and phone verification code.
- Personal and organization identity switching.
- API key list with pagination, usage/remain display, full-key reveal, and copy action.
- Agent selection for Claude Code, Codex, OpenCode, OpenClaw, and Hermes.
- One active key per agent across personal/organization identities.
- Local agent config writing with `sk-` prefixed TokenRouter API keys.
- In-app update check, download, and restart-to-update flow.
- GitHub Releases based update metadata for manual release publishing.

## Supported Agents

TokenRouter Switch checks whether the corresponding local agent directory exists before applying a key. If the agent is not installed, the app shows a dialog asking the user to install it first.

| Agent | Local config path | Behavior |
| --- | --- | --- |
| Claude Code | `~/.claude/settings.json` | Writes `ANTHROPIC_BASE_URL=https://api.tokenrouter.com` and `ANTHROPIC_AUTH_TOKEN=sk-...`. Creates the file with default env values if missing. |
| Codex | `~/.codex/config.toml`, `~/.codex/auth.json` | Ensures TokenRouter provider exists in `config.toml`, then overwrites `auth.json` with the selected `OPENAI_API_KEY`. |
| OpenCode | macOS/Linux: `~/.config/opencode/opencode.json`; Windows: `%LOCALAPPDATA%\\opencode\\opencode.json` | Updates an existing TokenRouter provider or lets the user select a model and writes a TokenRouter provider config. |
| OpenClaw | `~/.openclaw/openclaw.json` | Updates an existing TokenRouter provider. If missing, fetches available models, asks the user to choose one, then runs the OpenClaw CLI config commands. |
| Hermes | `~/.hermes/config.yaml` | Updates an existing TokenRouter config. If missing, fetches available models, asks the user to choose one, then writes TokenRouter base URL, key, and model config. |

## User Guide

1. Open TokenRouter Switch.
2. Sign in with TokenRouter credentials.
3. Choose `Personal` or `Organization` in the top-right identity switcher.
4. Select an enabled API key from the list.
5. Click one or more agent icons in the `Active Application` column.
6. If a model selection dialog appears, choose a model and confirm.
7. The app writes the selected key to the agent's local config and shows a success dialog.

Disabled and expired keys are hidden from the key list.

## Security Notes

- API keys are fetched only when needed for reveal, copy, or agent application.
- The app stores login session data under Electron's user data directory and uses Electron `safeStorage` when available.
- The renderer process does not directly persist TokenRouter tokens.
- Agent config files receive the full API key with an `sk-` prefix.
- Do not publish private credentials, signed certificates, Apple notarization passwords, or GitHub tokens in this repository.

## Development

### Requirements

- Node.js 22 or newer is recommended.
- npm.
- macOS is required to build macOS `.dmg` and `.zip` artifacts.
- Windows is recommended for official Windows signing and packaging, although the current project can build a Windows NSIS package from macOS.

### Install

```bash
npm install
```

### Run in development

```bash
npm run dev
```

### Test

```bash
npm test
```

### Production build

```bash
npm run build
```

This generates the Electron/Vite output under `out/`.

## Packaging

The app is packaged with `electron-builder`.

### macOS

```bash
npm run dist:mac
```

Expected important outputs:

```text
release/TokenRouter-Switch-<version>-mac-arm64.dmg
release/TokenRouter-Switch-<version>-mac-arm64.dmg.blockmap
release/TokenRouter-Switch-<version>-mac-arm64.zip
release/TokenRouter-Switch-<version>-mac-arm64.zip.blockmap
release/latest-mac.yml
```

### Windows

```bash
npm run dist:win
```

Expected important outputs:

```text
release/TokenRouter-Switch-<version>-win-x64.exe
release/TokenRouter-Switch-<version>-win-x64.exe.blockmap
release/latest.yml
```

## GitHub Releases Update Channel

The app checks updates from the public GitHub Releases repository:

```text
yb98k999/tokenrouter-switch-releases
```

This repository can be a release-only public repository. It does not need to contain the source code.

Current publish config:

```json
{
  "provider": "github",
  "owner": "yb98k999",
  "repo": "tokenrouter-switch-releases",
  "releaseType": "release"
}
```

## Manual Release Process

The project does not require GitHub Actions. You can build locally and upload release assets manually.

### 1. Update version

Edit `package.json`:

```json
{
  "version": "0.1.1"
}
```

Use SemVer and keep the GitHub Release tag aligned with the package version:

```text
v0.1.1
```

### 2. Build artifacts

```bash
npm test
npm run build
npm run dist:mac
npm run dist:win
```

### 3. Prepare upload folder

Copy only the release assets required by auto-update:

```bash
VERSION=0.1.1
rm -rf "release-upload/v$VERSION"
mkdir -p "release-upload/v$VERSION"

cp \
  "release/TokenRouter-Switch-$VERSION-mac-arm64.dmg" \
  "release/TokenRouter-Switch-$VERSION-mac-arm64.dmg.blockmap" \
  "release/TokenRouter-Switch-$VERSION-mac-arm64.zip" \
  "release/TokenRouter-Switch-$VERSION-mac-arm64.zip.blockmap" \
  "release/TokenRouter-Switch-$VERSION-win-x64.exe" \
  "release/TokenRouter-Switch-$VERSION-win-x64.exe.blockmap" \
  "release/latest-mac.yml" \
  "release/latest.yml" \
  "release-upload/v$VERSION/"

shasum -a 256 "release-upload/v$VERSION"/* > "release-upload/v$VERSION/SHA256SUMS.txt"
```

Do not upload `release/mac-arm64`, `release/win-unpacked`, or `release/builder-debug.yml`.

### 4. Create GitHub Release

If using GitHub CLI:

```bash
gh auth login
gh release create "v0.1.1" "release-upload/v0.1.1"/* \
  --repo yb98k999/tokenrouter-switch-releases \
  --title "TokenRouter Switch v0.1.1" \
  --notes "TokenRouter Switch v0.1.1 release."
```

If uploading through the GitHub web UI:

1. Open `https://github.com/yb98k999/tokenrouter-switch-releases/releases/new`.
2. Set the tag to `v0.1.1`.
3. Set the release title to `TokenRouter Switch v0.1.1`.
4. Upload all files from `release-upload/v0.1.1`.
5. Publish the release only after all files are uploaded.

The `latest.yml` and `latest-mac.yml` files must be uploaded with the installers and blockmaps. Without these metadata files, in-app updates will not work.

## First Release

The first generated release version is:

```text
v0.1.0
```

Prepared upload folder:

```text
release-upload/v0.1.0
```

Files:

```text
TokenRouter-Switch-0.1.0-mac-arm64.dmg
TokenRouter-Switch-0.1.0-mac-arm64.dmg.blockmap
TokenRouter-Switch-0.1.0-mac-arm64.zip
TokenRouter-Switch-0.1.0-mac-arm64.zip.blockmap
TokenRouter-Switch-0.1.0-win-x64.exe
TokenRouter-Switch-0.1.0-win-x64.exe.blockmap
latest-mac.yml
latest.yml
SHA256SUMS.txt
```

Upload command:

```bash
gh auth login
gh release create v0.1.0 release-upload/v0.1.0/* \
  --repo yb98k999/tokenrouter-switch-releases \
  --title "TokenRouter Switch v0.1.0" \
  --notes "Initial TokenRouter Switch release."
```

## Code Signing and Notarization

The current local macOS build can be created without a Developer ID certificate, but official distribution should use signing and notarization.

Recommended production setup:

- macOS: Apple Developer ID Application certificate.
- macOS: notarization credentials.
- Windows: Authenticode code signing certificate.
- Never commit certificate files or signing credentials.

Unsigned builds are useful for internal testing, but users may see operating system security warnings.

## Troubleshooting

### Update check does not find a new version

- Confirm the app is running a packaged build, not `npm run dev`.
- Confirm the GitHub Release is public.
- Confirm the release tag version is higher than the installed `package.json` version.
- Confirm `latest.yml` and `latest-mac.yml` are uploaded.
- Confirm file names inside the YAML files exactly match the uploaded asset names.

### GitHub Release upload succeeds but download fails

- Re-upload all installer, blockmap, and latest metadata files together.
- Avoid editing `latest*.yml` manually unless the file names and hashes are fully understood.
- Do not rename installer files after `electron-builder` generates them.

### macOS says the app cannot be opened

- The build may be unsigned or not notarized.
- For internal testing, users may need to approve the app in macOS Privacy & Security.
- For production, sign and notarize the app.

### Agent application fails

- Make sure the target agent is installed.
- Make sure the expected config directory exists:
  - Claude Code: `~/.claude`
  - Codex: `~/.codex`
  - OpenClaw: `~/.openclaw`
  - OpenCode: `~/.config/opencode` on macOS/Linux or `%LOCALAPPDATA%\\opencode` on Windows
  - Hermes: `~/.hermes`
- For OpenClaw, make sure the `openclaw` command is available in PATH.

## Project Structure

```text
src/main       Electron main process, auth requests, session storage, update logic, agent config writers
src/preload    Safe IPC bridge exposed to the renderer
src/renderer   React UI
src/shared     Shared TypeScript types
tests          Unit and integration-style tests
assets         App icon assets
release        electron-builder output
release-upload Release-only upload folders
```

## License

Proprietary. All rights reserved.
