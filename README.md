# Downloader (dl)

A standalone Linux utility for unpacking archives and installing packages. It automates extraction, runs installation scripts, and handles package installation with privilege elevation when needed.

## Quick Start: 
1) Install dependencies:
# Arch Linux
sudo pacman -S erlang erlang-escript

# Debian/Ubuntu 
sudo apt update && sudo apt install erlang-base erlang-escript

# Fedora/RHEL/CentOS
sudo dnf install erlang

# openSUSE
sudo zypper install erlang

# Alpine Linux
sudo apk add erlang erlang-escript

2) Install the project file, you need to make it executable

3) In the folder with the file, you can run the commands: ./dl --gui | ./dl --check | ./dl --help

## Quick Summary

- **Usage:** `./dl <file_path_or_URL>`
- **No Dependencies:** Runs as a single binary (does not require Elixir/Erlang).
- **Automation Features:**
    - **Extracts:** `.tar`, `.tar.gz/.tgz`, `.tar.xz/.txz`, `.tar.bz2/.tbz2`, `.tar.zst/.tzst`, `.zip`, `.7z`, `.rar`, and single `.gz/.xz/.bz2/.zst` files.
    - **Installs:** Automatically detects and runs `install.sh`, `setup.py`, or `make && make install`.
    - **Packages:** Installs `.deb`, `.rpm`, `.snap`, and `.flatpak` (using pkexec/sudo); extracts `.AppImage` content; saves `.flatpakref` as a reference; unpacks `.pkg.tar.zst` and `.apk`.

## Supported Formats

- **Archives:** `.tar`, `.tar.gz`, `.tgz`, `.tar.xz`, `.txz`, `.tar.bz2`, `.tbz2`, `.tar.zst`, `.tzst`, `.zip`, `.7z`, `.rar`, `.gz`, `.xz`, `.bz2`, `.zst`
- **Packages:** `.deb`, `.rpm`, `.AppImage` (extraction), `.snap`, `.flatpak`, `.flatpakref` (reference), `.pkg.tar.zst` (Pacman), `.apk` (Alpine), `.run/.bin` (extraction attempt).

## Requirements

- **OS:** Linux x86_64 with standard glibc (most modern distributions).
- **System Tools:** Based on the file format:
    - **Archives:** `tar`, `unzip`.
    - **.deb:** `dpkg` (and `apt-get` for dependencies).
    - **.rpm:** `dnf`, `yum`, `zypper`, or `rpm`.
    - **.snap:** `snap`.
    - **.flatpak:** `flatpak`.
- **Permissions:** Root/sudo access is required to install system packages (`.deb`, `.rpm`, `.snap`).

## Examples

```bash
# Extract an archive and attempt installation
./dl /path/app.tar.gz
./dl /path/app.tar.xz
./dl /path/app.zip

# Install a system package (will prompt for password)
./dl /path/app.deb
./dl /path/app.rpm

# AppImage — extract content
./dl /path/App.AppImage

# Snap (local .snap file)
./dl /path/app.snap

# Flatpak bundle (local .flatpak file)
./dl /path/app.flatpak

# Download and process from URL
./dl https://example.com/file.zip
```

## Default Behavior

- **Location:** Files are unpacked into the same directory where the archive is located.
- **Auto-Install Script Search:** After extraction, the utility looks up to 3 levels deep for:
    - `install.sh`: Executed via `bash`.
    - `setup.py`: Installed via `python3 install` (uses `--user` if not root).
    - `Makefile`: Runs `make` (and `make install` if root).
- **AppImage:** Adds execution permissions and extracts content.

## Tips and Security

- **Trust:** Only process files from trusted sources. This utility may execute scripts contained within archives.
- **Permissions:** If you encounter permission errors, use `sudo` (only when necessary).
- **Missing Tools:** If a specific format fails (e.g., `dpkg` not found), install the required system tool using your distribution's package manager.

## Troubleshooting

- **"File not found":** Verify the path. Use quotes if the path contains spaces.
- **"Unsupported or unknown archive type":** The file extension is not recognized.
- **Using .deb on Arch/Manjaro:** The utility will block this to prevent system conflicts. Use native tools or conversion utilities instead.
- **Package Manager Errors:** Check the terminal output for specific errors like missing dependencies or network issues.

## License

MIT (unless otherwise stated).
