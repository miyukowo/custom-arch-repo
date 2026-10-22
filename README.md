# Custom Arch Linux Repository (`custom-arch-repo`)

Automated personal Pacman repository for **Google Antigravity**, built automatically with GitHub Actions.

## How It Works
1. A GitHub Actions workflow runs once daily at midnight UTC (and on manual trigger).
2. It checks `https://antigravity.google/download` for new versions and build IDs.
3. If an update is detected, it automatically packages the latest Antigravity binary into an Arch Linux package (`.pkg.tar.zst`) using an official `archlinux:base-devel` container, updates the `repo-add` repository database, and publishes it as a GitHub Release under the `x86_64` tag.
4. Packages and repository databases are dual-published to:
   - **Cloudflare R2** (accelerated globally via Cloudflare CDN edge caching)
   - **GitHub Releases** (fallback mirror under the `x86_64` tag)
5. Pacman synchronizes with Cloudflare CDN for maximum download speed, automatically falling back to GitHub Releases if needed.

---

## Setup Instructions

### 1. Add Repository to `/etc/pacman.conf`

Open `/etc/pacman.conf` with root privileges:

```bash
sudo nano /etc/pacman.conf
```

Append the following configuration to the bottom (replace `arch.example.com` with your custom domain):

```ini
[custom]
SigLevel = Optional TrustAll
# Primary mirror (Cloudflare R2 CDN - High speed):
Server = https://arch.example.com/x86_64
# Fallback mirror (GitHub Releases):
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

---

## Repository Secrets Configuration
To enable the Cloudflare R2 sync step, set the following GitHub Secrets (**Settings** → **Secrets and variables** → **Actions**):

| Secret | Description |
| --- | --- |
| `R2_ACCESS_KEY_ID` | R2 S3 API Access Key ID |
| `R2_SECRET_ACCESS_KEY` | R2 S3 API Secret Access Key |
| `R2_ENDPOINT_URL` | `https://<ACCOUNT_ID>.r2.cloudflarestorage.com` |
| `R2_BUCKET_NAME` | *(Optional)* Target R2 bucket name (defaults to `arch-repo`) |
