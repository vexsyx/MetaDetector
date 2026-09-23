# Meta Detector

Meta Detector is a focused Sol's RNG biome and aura detector with Discord webhooks and anti-AFK support. Version `1.0.0` uses the existing Oyster Detector remote biome, aura, rank, and asset data.

Meta Detector was commissioned by **K. Saiko** (Discord ID `481491983999959040`).

The interface keeps Oyster Detector's tabbed layout and inline biome/aura controls, rebuilt with CustomTkinter. Appearance mode, CustomTkinter color theme, and py-window-styles window chrome are configurable from Settings.

## Development

```powershell
py -m pip install -r requirements.txt
py main.py
```

The application stores settings, statistics, logs, cached remote data, and updates in `%LOCALAPPDATA%\MetaDetector`.

## Build

```powershell
py build.py
```

The build runs through PyArmor before PyInstaller. Because the unregistered
PyArmor build has a big-script limit, `build.py` stores the compiled main app in
an encrypted payload and PyArmor-protects the loader/key; the bootstrapper and
shared update helper are obfuscated directly. A licensed PyArmor installation
can also be used by the same build environment.

This produces:

- `dist\MetaDetector.exe` — the standalone application.
- `dist\MetaDetector-Bootstrapper.exe` — the small download/update launcher users should download.

Both executables use `meta.png` for their packaged icon. The bootstrapper installs the app as `%LOCALAPPDATA%\MetaDetector\app.exe` and creates a per-user Start Menu shortcut named **Meta Detector**, which makes it discoverable in Windows Search. No Python packaging or obfuscation system can make extraction mathematically impossible, but this avoids distributing the app as ordinary recoverable PyInstaller bytecode.

## Releases and independent updates

Update `data\update-manifest.json`, then publish it on the `main` branch. Each component has its own version, download URL, and optional SHA-256 digest, so the bootstrapper can update without changing the app version.

Recommended release assets:

- `MetaDetector.exe`
- `MetaDetector-Bootstrapper.exe`

For production releases, fill in each `sha256` value before publishing the manifest. The app checks the `app` entry; the bootstrapper checks both entries, replaces itself when its own version changes, downloads or updates `app.exe`, then launches it.

If the manifest is unavailable, the updater falls back to the latest GitHub release and looks for the standard asset names.
