# Pelican Modpack Manager (v1.7.0)

Browse, install, update, and switch Minecraft modpacks directly from the Pelican server panel.

Supports **CurseForge, Modrinth, FTB, and ATLauncher**, with automatic handling for **Forge, NeoForge, Fabric, and Quilt**. If the server does not already have the loader egg a selected pack needs, Modpack Manager can pull the current official egg from Pelican's `pelican-eggs/minecraft` repository, switch the server to it, configure the loader/Minecraft/Java settings, install the runtime, and prepare the pack automatically.

The browser includes descriptions, galleries, project links, pagination, multi-loader/category filters, provider-failure recovery, and a final install confirmation. Individual **Mods / Plugins** have the same description/gallery/project-link and paging/filter experience.

---

## 🚀 What's New in v1.7.0

- **⚡ Turbo Download Engine (24+ MB/s)**:
  Bypasses CurseForge / CloudFront CDN rate-limiting (which caps Go-based HTTP clients at ~280 kB/s). Utilizing browser header emulation and direct node upload streaming, 1.2 GB+ modpacks download in seconds rather than 70+ minutes.
- **🐧 Linux Shell Script Sanitizer**:
  Automatically normalizes Windows CRLF (`\r\n`) to Unix LF (`\n`) on all extracted bash scripts (`startserver.sh`, `run.sh`, `start.sh`, `Install.sh`), completely eliminating `/bin/bash^M: bad interpreter` container crashes.
- **⏱️ Extended Timeout Protections**:
  - Queue job execution timeout increased to **7,200 seconds (2 hours)**.
  - Download watchdog and stale install checks extended to **3,600 seconds (1 hour)**, ensuring large packs like *All The Mods 10* never time out during download or initialization.
- **⚙️ Admin Turbo Controls**:
  Added an administrative toggle in Panel Settings and `config/modpack-manager.php` (`MODPACK_MANAGER_TURBO_DOWNLOAD`) to configure or disable Turbo mode if needed.

---

## 📦 Features

- **Multi-Platform Support**: Seamless browsing across CurseForge, Modrinth, Feed The Beast (FTB), and ATLauncher.
- **Automated Loader & Egg Switching**: Auto-detects loader requirements (Forge, NeoForge, Fabric, Quilt) and assigns the official Pelican egg.
- **Smart Java Auto-Configuration**: Detects required Java versions (Java 8, 11, 17, 21) based on Minecraft and loader metadata.
- **Safe Pack Switching**:
  - Automatic backup creation prior to installation.
  - Intelligent preservation of player data, whitelists, ops, EULA, and world directories (`level.dat`).
  - Cleans up legacy loader files and stale startup scripts to prevent conflict.
- **Real-Time Progress & Logs**: Live polling step updates and readable debug logs directly in the panel UI.

---

## 🛠️ Installation

1. Download or clone this repository into your Pelican plugins directory:
   ```bash
   cd /var/www/pelican/plugins
   git clone https://github.com/sadmaxie/pelican-modpackmanager.git modpack-manager
   chown -R www-data:www-data modpack-manager
   ```

2. Install and run database migrations:
   ```bash
   cd /var/www/pelican
   php artisan p:plugin:install modpack-manager
   ```

3. Clear caches and restart queue workers:
   ```bash
   php artisan optimize:clear
   php artisan queue:restart
   ```

> [!NOTE]
> If you are running Pelican in Docker, you can copy the plugin folder into the container:
> ```bash
> docker cp modpack-manager pelican-panel:/var/www/html/plugins/
> docker exec -it pelican-panel chown -R www-data:www-data /var/www/html/plugins/modpack-manager
> docker exec -it pelican-panel php artisan optimize:clear
> docker exec -it pelican-panel php artisan queue:restart
> ```

---

## 📄 License & Notes

- For detailed developer notes and historical change logs, see [`NOTES.md`](NOTES.md).
- Released under the [MIT License](LICENSE).
