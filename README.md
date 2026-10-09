# Custom Arch Linux Repository (`custom-arch-repo`)

Automated personal Pacman repository for **Google Antigravity**, built automatically with GitHub Actions.

## How It Works
1. A GitHub Actions workflow runs once daily at midnight UTC (and on manual trigger).
2. It checks `https://antigravity.google/download` for new versions and build IDs.
3. If an update is detected, it automatically packages the latest Antigravity binary into an Arch Linux package (`.pkg.tar.zst`) using an official `archlinux:base-devel` container, updates the `repo-add` repository database, and publishes it as a GitHub Release under the `x86_64` tag.
4. Pacman synchronizes directly with GitHub Releases just like any standard Arch Linux repository.

---

## Setup Instructions

### 1. Add Repository to `/etc/pacman.conf`

Open `/etc/pacman.conf` with root privileges:

```bash
sudo nano /etc/pacman.conf
```

Append the following configuration to the bottom:

```ini
[custom]
SigLevel = Optional TrustAll
Server = https://github.com/miyukowo/custom-arch-repo/releases/download/x86_64
```
*(Alternatively, you can also use `[antigravity]` as the section header).*

### 2. Update and Install

Synchronize your package database and install or upgrade Antigravity:

```bash
sudo pacman -Syu antigravity
```

If you previously installed an AUR package like `antigravity-appimage`, `pacman` will automatically offer to replace it.

---

## Manual Workflow Trigger
To force an immediate check and build:
1. Go to the **Actions** tab on GitHub.
2. Select **Auto-Build Antigravity Pacman Repo**.
3. Click **Run workflow**.
