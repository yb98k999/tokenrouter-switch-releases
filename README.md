# TokenRouter Switch

TokenRouter Switch is a desktop app for managing TokenRouter API keys and quickly applying them to local AI coding agents.

It helps you switch the active API key used by Claude Code, Codex, OpenCode, OpenClaw, and Hermes without manually editing local configuration files.

## What You Can Do

- Sign in to your TokenRouter account.
- Switch between personal and organization identities.
- View enabled TokenRouter API keys in one place.
- Reveal or copy a full API key when needed.
- Apply one API key to supported local agents.
- Keep one active key per agent, even when switching between personal and organization identities.
- Check for new versions directly inside the app.

## Supported Sign-In Methods

TokenRouter Switch supports:

- Email and password sign-in.
- Phone verification code sign-in.
- Personal and organization identity switching after sign-in.

## API Key Management

The API key list shows enabled TokenRouter keys for the currently selected identity.

For each key, you can:

- View its name and masked key value.
- Reveal the full key.
- Copy the full key with the `sk-` prefix.
- View usage and remaining quota.
- Select which local agents should use that key.

Disabled and expired keys are hidden from the list.

## Supported Agents

TokenRouter Switch can apply a selected TokenRouter API key to:

- Claude Code
- Codex
- OpenCode
- OpenClaw
- Hermes

When you select an agent, the app first checks whether that agent appears to be installed locally. If it cannot find the expected local folder, it will ask you to install the agent first.

For agents that require a model choice, TokenRouter Switch will show a model selection dialog before applying the configuration.

## Agent Switching Behavior

Each supported agent can have only one active API key at a time.

For example:

- If `Claude Code` is already assigned to Key A, selecting `Claude Code` on Key B automatically moves `Claude Code` from Key A to Key B.
- Clicking an already selected agent does not cancel it.
- A single API key can be active for multiple agents at the same time.

The selected agent state is preserved locally so your choices remain visible after switching identities or reopening the app.

## In-App Updates

TokenRouter Switch includes a built-in update button.

The update flow supports:

- Checking for new versions.
- Downloading available updates.
- Restarting the app to install the downloaded update.

Updates are distributed through the public GitHub Releases channel:

```text
yb98k999/tokenrouter-switch-releases
```

Only release files are published there. The application source code does not need to be public.

## Privacy and Security

- TokenRouter Switch uses your TokenRouter login session to load your account, identities, API keys, and available models.
- Full API keys are fetched only when needed, such as revealing, copying, or applying a key to an agent.
- Login session data is stored locally by the desktop app.
- API keys are written only to the selected local agent configuration when you choose to apply them.
- The app does not require you to manually copy API keys into agent configuration files.

## Platform Support

TokenRouter Switch is intended for:

- macOS
- Windows

The first generated release includes:

- macOS Apple Silicon package.
- Windows x64 installer.

## First Release

Current first release version:

```text
v0.1.0
```

Release assets are published through GitHub Releases and include the installer files required for app updates.

## Notes

- For best results, install the target agent before applying an API key to it.
- Some operating systems may show a security warning for unsigned or newly released desktop apps.
- If an update is available but cannot be installed, download the latest installer from the GitHub Releases page and install it manually.
