# Upify
> [!Warning]
> It's a vibe coded project

[![Release](https://img.shields.io/github/v/release/Matheotaku4/Upify?color=blue&logo=github)](https://github.com/Matheotaku4/Upify/releases)
[![Node](https://img.shields.io/badge/Node.js-%3E%3D_18-339933?logo=node.js&logoColor=white)](https://nodejs.org/)
[![Electron](https://img.shields.io/badge/Electron-47848F?logo=electron&logoColor=white)](https://www.electronjs.org/)
[![Windows](https://img.shields.io/badge/Windows-0078D6?logo=windows&logoColor=white)](#)
[![Linux](https://img.shields.io/badge/Linux-FCC624?logo=linux&logoColor=black)](#)
[![Stars](https://img.shields.io/github/stars/Matheotaku4/Upify?color=gold)](https://github.com/Matheotaku4/Upify/stargazers)

![Screenshot](https://files.catbox.moe/4728qb.jpg)
Local desktop/web uploader for sending one file to multiple hosts in parallel:

- [gofile.io](https://gofile.io)
- [1fichier.com](https://1fichier.com)
- [rootz.so](https://rootz.so)
- [send.now](https://send.now)
- [buzzheavier.com](https://buzzheavier.com)
- [vikingfile.com](https://vikingfile.com)
- [filemirage.com](https://filemirage.com)
- [pixeldrain.com](https://pixeldrain.com)
- [bowfile.com](https://bowfile.com)
- [akirabox.to](https://akirabox.to) (Premium API key required)
- [0807.st](https://0807.st)
- [dropdrive.rip](https://dropdrive.rip)

## Requirements

- Node.js `>= 18`
- npm

## Installation

Download the latest binaries from Releases:

[![Release](https://img.shields.io/github/v/release/Matheotaku4/Upify?color=blue&logo=github)](https://github.com/Matheotaku4/Upify/releases)

Or clone and run locally:

```bash
git clone https://github.com/Matheotaku4/Upify.git
cd Upify
npm install
npm start
```

Then open:

```text
http://localhost:3000
```

## Linux

Releases include a portable AppImage and a Debian package:

```bash
# AppImage (no installation needed)
chmod +x Upify-<version>-x86_64.AppImage
./Upify-<version>-x86_64.AppImage

# Debian / Ubuntu
sudo apt install ./Upify-<version>-amd64.deb
```

The AppImage needs FUSE; on a minimal system without it, run it with
`--appimage-extract-and-run` or install `libfuse2`.

## Desktop Build

Windows (portable + installer):

```bash
npm run build:release
```

Linux (AppImage + deb):

```bash
npm run build:linux
```

Main outputs:

```text
dist-electron/Upify-<version>-portable.exe
dist-electron/Upify-<version>-setup.exe
dist-electron/Upify-<version>-x86_64.AppImage
dist-electron/Upify-<version>-amd64.deb
```

## Releases

The `Release` GitHub Actions workflow builds both platforms and attaches every
artifact to a GitHub release. It runs on a `v*` tag push or manually from the
Actions tab (it creates the tag and the release from the version in
`package.json`).

## Documentation

- [API Guide](./API.md): where to get API keys/tokens and whether they are required.

## Features

- Per-host parallel upload with detailed result cards.
- Per-host progress bars and host-level cancellation.
- Global "Cancel all" for a full running job.
- "Copy all links" and "Save links (.txt)" actions after upload.
- Persistent preferences: selected hosts, API fields, advanced panel state.
- Upload stats counter: "You have uploaded x Go".
- Desktop mode: local server started in background with an integrated Electron window.
- Windows notifications when upload is complete.
- External links open in your default browser.
- Native Windows and Linux desktop builds.
- 0807.st support: anonymous uploads with the required proof-of-work, optional
  partner token, expiry and password, automatic chunked upload for files above 2 GB.
- DropDrive support: anonymous uploads with the required proof-of-work, optional
  account token, and chunked multi-part uploads.
- BowFile integration via `/api/v2/authorize` + `/api/v2/file/upload`.
- AkiraBox integration via the current `akirabox.to` signed-upload API.

## Local API

### `POST /api/upload`

`multipart/form-data` fields:

- `file`: uploaded file
- `targets`: JSON array (example: `["gofile","1fichier"]`)
- `options`: JSON object keyed by target (optional)

Example:

```json
{
  "gofile": { "token": "..." },
  "1fichier": { "apiKey": "..." },
  "rootz": { "folderId": "..." },
  "buzzheavier": { "locationId": "3eb9t1559lkv" },
  "sendnow": { "link_rcpt": "mail@example.com" },
  "vikingfile": { "user": "" },
  "filemirage": { "apiToken": "..." },
  "pixeldrain": { "apiKey": "..." },
  "bowfile": { "key1": "...", "key2": "...", "folderId": "optional" },
  "akirabox": { "apiKey": "...", "otp": "optional 2FA code" },
  "0807st": { "token": "optional partner token", "expiry": "24", "password": "optional" },
  "dropdrive": { "token": "optional account token" }
}
```
