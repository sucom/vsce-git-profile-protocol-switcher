# Git Profile-Protocol Switcher

<p align="center">
  <img src="icon-sm.png" alt="Git Profile-Protocol Switcher" width="128"/>
</p>

A lightning-fast, fully offline utility to visualize and switch your Git user profiles and remote transport protocols (HTTPS/SSH) directly from the VS Code status bar.

Built on the KISS paradigm, this extension relies entirely on your local `.git/config` and `~/.ssh/config`—zero external API calls, zero authentication friction, and zero background bloat. Perfect for developers, architects, and bootcamp educators managing multiple organizational identities.

## ✨ Key Features

* **Instant Visibility**: Two unobtrusive status bar icons display your active Git identity (Name/Email) and your current remote transport protocol (HTTPS/SSH) for the active workspace.
* **Frictionless Profile Switching**: Click the identity icon to select a saved profile. The extension instantly updates the local repository's `user.name` and `user.email`.
* **Integrated Creation Flow**: Add new profiles on the fly directly from the Quick Pick menu.
* **Smart Protocol Routing**: Click the transport icon to instantly toggle your `origin` remote between standard HTTPS and your configured SSH aliases.
* **Auto-Parses SSH Config**: Automatically reads `~/.ssh/config` to provide a unified, flattened list of all your available SSH hosts.
* **Deep Context Awareness**: Silently adapts to the currently active editor or workspace folder, checking the native Git tree to ensure complete accuracy.

## 🚀 Usage

### Managing Identity (Profiles)
1. Look at the bottom-left status bar for the **Account Icon** (e.g., `$(account) Work` or `$(question) Unknown`).
2. Click the icon to open the Profile Quick Pick menu.
3. Select an existing profile to apply it locally, or select **Create New Profile...** to instantly configure and save a new identity.

### Managing Transport (HTTPS / SSH)
1. Look at the bottom-left status bar for the **Lock/Key Icon** (e.g., `$(lock) HTTPS` or `$(key) SSH`).
2. Click the icon to open the Transport Quick Pick menu.
3. Select your desired transport. The extension will safely execute `git remote set-url origin` to seamlessly route your connection.

## ⚙️ Configuration

Profiles are stored in plain text in your global `settings.json`, making them easy to manage, sync, and audit.

```json
"gitProfileProtocolSwitcher.profiles": [
  {
    "label": "Personal",
    "name": "Jane Doe",
    "email": "jane@example.com"
  },
  {
    "label": "CoreBootcamp",
    "name": "Jane Doe (Instructor)",
    "email": "educator@corebootcamp.com"
  }
]
```

To edit or remove profiles, simply click **Manually manage profiles...** from the status bar menu, or manually open your VS Code Settings and search for `Git Profile-Protocol Switcher`.

## 🔒 Privacy & Security

This extension is completely offline. It reads your local `~/.ssh/config` strictly to parse hostnames for routing and sets local `.git/config` variables. It does not read, require, or transmit Personal Access Tokens (PATs), passwords, or repository contents.

## 🚀 Installation

- Install from [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=spajs.git-profile-protocol-switcher)

- Command Line: `code --install-extension spajs.git-profile-protocol-switcher`

OR

1. Open VS Code
2. Go to Extensions
3. Search for `Git Profile-Protocol Switcher` and select this extension
4. Click **Install**


##### Other related extensions

- [Git Snapshots](https://marketplace.visualstudio.com/items?itemName=spajs.git-snapshots) - For Git based snapshots.
- [Git Pull Agent](https://marketplace.visualstudio.com/items?itemName=spajs.git-pull-agent) - For pulling changes from remote repo.
- [Backup File](https://marketplace.visualstudio.com/items?itemName=spajs.backup-file) - For simple file snapshots without custom tags.
- [Tagged File Snapshots](https://marketplace.visualstudio.com/items?itemName=spajs.tagged-file-snapshots) - For simple file snapshots with tags.
- [Tagged Snapshots](https://marketplace.visualstudio.com/items?itemName=SPAjs.tagged-snapshots) - For Tagged snapshots of editor files (all/active tab group)
- [Backup Folder](https://marketplace.visualstudio.com/items?itemName=spajs.backup-folder) - To backup/snapshot a folder.

## ☑️ Requirements

- VS Code v1.75.0 or higher.
- Git

## ⚖️ License

MIT

## 🏠 Home

[GitHub](https://github.com/sucom/vsce-git-profile-protocol-switcher)
