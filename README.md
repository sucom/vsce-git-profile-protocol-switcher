# Git Profile-Protocol Switcher (VS Code Family Extension)

<p>
  <img src="icon-sm.png" alt="Git Profile-Protocol Switcher" width="128"/>
</p>

A lightning-fast, fully offline utility to visualize and switch your Git user profiles and remote transport protocols (HTTPS/SSH) directly from the VS Code status bar.

Built on the KISS paradigm, this extension relies entirely on your local `.git/config` and `~/.ssh/config`—zero external API calls, zero authentication friction, and zero background bloat. Perfect for developers, architects, and educators managing multiple organizational identities.

## ✨ Key Features

* **Instant Visibility**: Two unobtrusive status bar icons display your active Git identity (Name/Email) and your current remote transport protocol (HTTPS/SSH) for the active workspace.
* **Frictionless Profile Switching**: Click the identity icon to select a saved profile. The extension instantly updates the local repository's `user.name` and `user.email`.
* **Integrated Creation Flow**: Add new profiles on the fly directly from the Quick Pick menu.
* **Smart Protocol Routing**: Click the transport icon to instantly toggle your `origin` remote between standard HTTPS and your configured SSH aliases. Fully supports GitHub, GitLab, Bitbucket, and custom Git servers.
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

~~~json
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
~~~

To edit or remove profiles, simply click **Manually manage profiles...** from the status bar menu, or manually open your VS Code Settings and search for `Git Profile-Protocol Switcher`.

## 🔐 Git SSH Authentication Tips

> Checkout the companion extension **`Git SSH Config Manager`** -
  [VS Marketplace](https://marketplace.visualstudio.com/items?itemName=spajs.git-ssh-config-manager)
  | [Open VSX Registry](https://open-vsx.org/extension/SPAjs/git-ssh-config-manager) for managing SSH Config for Git.
>
> **⚠️ IMPORTANT NOTE:** *The configurations below are fully automated by **Git SSH Config Manager** extension. This informational guide is provided so you can understand the underlying mechanics and validate your setup—especially when you want to juggle between multiple accounts across different Git providers.*

### SSH Keys for multiple identities

> Create an `.ssh` folder in `~` (system home folder) if it does not exist.
>
> In Windows: `~` = `C:\Users\YourName`

1. Generate an *ssh key file* in the `~/.ssh` folder, using the `ssh-keygen` command:

~~~shell
ssh-keygen -t ed25519 -a 100 -C "github-key-1" -f ~/.ssh/id_ed25519-github-key1
~~~

This will generate:

- *Private key:* `~/.ssh/id_ed25519-github-key1`
- *Public key:* `~/.ssh/id_ed25519-github-key1.pub`

> `-t ed25519` → Algorithm: fixed and recommended.
>
> `-a 100` → KDF Rounds (optional): Makes local brute-force 100x harder.
>
> `-C "github-key-1"` → Comment: human‑readable label (short text/email) for easy identification inside the key file. Ex: "GitHub-key-1", "work", "email-address", etc.
>
> `-f ~/.ssh/id_ed25519-github-key1` → File path and name for the key pair. Recommended descriptive filenames. Ex: `id_ed25519-github`, `id_ed25519-work`, etc.

2. Add the key to your repository provider's SSH settings (e.g., GitHub):

Copy the content of your public key `~/.ssh/id_ed25519-github-key1.pub` into GitHub (or your repository provider) → Settings → SSH and GPG keys.

#### *Optional* Configuration for multiple accounts [Personal, Work]:
Edit `~/.ssh/config`:

~~~text
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519-github-key1

Host bitbucket.company.com
  HostName bitbucket.company.com
  User git
  IdentityFile ~/.ssh/id_ed25519-work
~~~
> Use standard SSH repo URLs like:
>
> `git@github.com:usernameX/repo.git`
>
> `git@bitbucket.company.com:usernameY/repo.git`

#### Verify SSH setup

Run the following to confirm your key is working:

~~~shell
ssh -T git@github.com

# If successful, GitHub will reply:
# Hi usernameX! You've successfully authenticated, but GitHub does not provide shell access.

ssh -T git@bitbucket.company.com

# If successful, GitHub will reply:
# Hi usernameY! You've successfully authenticated, but GitHub does not provide shell access.
~~~

> You may get a first-time warning. Continue connecting by typing `yes`.<br>
>
> `The authenticity of host 'github.com (n.n.n.n)' can't be established.`<br>
> `ED25519 key fingerprint is SHA256:+...x...y...z...U.`<br>
> `This key is not known by any other names.`<br>
> `Are you sure you want to continue connecting (yes/no/[fingerprint])? yes`<br>
> `Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.`<br>
>
> This will create two files (`known_hosts`, `known_hosts.old`) in your `~/.ssh` folder.

#### Using Aliases (Multiple accounts on a single domain)

If you have multiple accounts on a single domain (e.g., GitHub), use aliases in your `~/.ssh/config`:

~~~text
Host github-userX
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519-github-key1

Host github-userY
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519-github-key2
~~~
> Define different Host aliases with respective IdentityFiles for each account.
>
> Then use repo URLs like:
>
> - `git@github-userX:usernameX/repo.git`
>
> - `git@github-userY:usernameY/repo.git`

#### Verify SSH setup with aliases
~~~shell
ssh -T git@github-userX
# If successful, GitHub will reply:
# Hi usernameX! You've successfully authenticated...

ssh -T git@github-userY
# If successful, GitHub will reply:
# Hi usernameY! You've successfully authenticated...
~~~

## 🔒 Privacy & Security

This extension is completely offline. It reads your local `~/.ssh/config` strictly to parse hostnames for routing and sets local `.git/config` variables. It does not read, require, or transmit Personal Access Tokens (PATs), passwords, or repository contents.

## 🚀 Installation

**Install via GUI:**
1. Open your IDE (VS Code, VSCodium, Antigravity, or Cursor)
2. Go to the Extensions view
3. Search for `Git Profile-Protocol Switcher` and select this extension
4. Click **Install**

**Install from Marketplace / VSX Registry:**
- [VS Marketplace](https://marketplace.visualstudio.com/items?itemName=spajs.git-profile-protocol-switcher)
- [Open VSX Registry](https://open-vsx.org/extension/SPAjs/git-profile-protocol-switcher)

**Install via Command Line:**
- For VS Code:
  ```shell
  code --install-extension spajs.git-profile-protocol-switcher
  ```
- For VSCodium:
  ```shell
  codium --install-extension spajs.git-profile-protocol-switcher
  ```
- For Google Antigravity:
  ```shell
  antigravity --install-extension spajs.git-profile-protocol-switcher
  ```
- For Cursor:
  ```shell
  cursor --install-extension spajs.git-profile-protocol-switcher
  ```

## ✨ Other Related Extensions

- Git SSH Config Manager -
  [VS Marketplace](https://marketplace.visualstudio.com/items?itemName=spajs.git-ssh-config-manager)
  | [Open VSX Registry](https://open-vsx.org/extension/SPAjs/git-ssh-config-manager)

- Git Snapshots -
  [VS Marketplace](https://marketplace.visualstudio.com/items?itemName=spajs.git-snapshots)
  | [Open VSX Registry](https://open-vsx.org/extension/SPAjs/git-snapshots)

- Git Pull Agent -
  [VS Marketplace](https://marketplace.visualstudio.com/items?itemName=spajs.git-pull-agent)
  | [Open VSX Registry](https://open-vsx.org/extension/SPAjs/git-pull-agent)


- Backup File -
  [VS Marketplace](https://marketplace.visualstudio.com/items?itemName=spajs.backup-file)
  | [Open VSX Registry](https://open-vsx.org/extension/SPAjs/backup-file)

- Tagged File Snapshots -
  [VS Marketplace](https://marketplace.visualstudio.com/items?itemName=spajs.tagged-file-snapshots)
  | [Open VSX Registry](https://open-vsx.org/extension/SPAjs/tagged-file-snapshots)

- Tagged Snapshots -
  [VS Marketplace](https://marketplace.visualstudio.com/items?itemName=SPAjs.tagged-snapshots)
  | [Open VSX Registry](https://open-vsx.org/extension/SPAjs/tagged-snapshots)

- Backup Folder -
  [VS Marketplace](https://marketplace.visualstudio.com/items?itemName=spajs.backup-folder)
  | [Open VSX Registry](https://open-vsx.org/extension/SPAjs/backup-folder)

## ☑️ Requirements

- VS Code v1.75.0 or higher
- Git installed and accessible in your system path

## ⚖️ License

MIT

## 🏠 Home

[GitHub](https://github.com/sucom/vsce-git-profile-protocol-switcher)
