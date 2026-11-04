# Upify API Guide

This page explains which Advanced Options in Upify need an API key or token, where to get it, and whether it is required.

## Quick Summary

| Host | Option in Upify | Required | Useful |
|---|---|---|---|
| `gofile.io` | `Gofile token` | No | Yes, to reuse your own account/folders |
| `1fichier.com` | `1fichier API key` | No | Yes, for account-linked uploads |
| `rootz.so` | `Rootz folderId` | No | Yes, to upload into a specific folder |
| `send.now` | `Send.now recipient` | No | Only if you want email recipient behavior |
| `buzzheavier.com` | `Buzzheavier locationId` | No | Optional, mostly advanced routing |
| `vikingfile.com` | `Vikingfile user` | No | Optional if you use a specific user/path flow |
| `filemirage.com` | `FileMirage API token` | No | Yes, if your account/server requires auth |
| `pixeldrain.com` | `Pixeldrain API key` | **Yes** | **Required for API upload** |
| `bowfile.com` | `BowFile Key 1 / Key 2` | **Yes** | **Required for authenticated uploads** |
| `akirabox.to` | `AkiraBox API key` | **Yes** | **Required, Premium account only** |
| `0807.st` | `0807.st partner token` | No | Skips proof-of-work and raises limits |
| `dropdrive.rip` | `DropDrive account token` | No | Stores uploads in your account instead of anonymously |

## Host-by-Host Details

### Gofile (`gofile.io`)
- Option: `Gofile token`
- Required: No
- If empty, Upify creates a temporary account automatically.
- If provided, uploads use your token and account context.

How to get it:
1. Log in to Gofile.
2. Open account/security settings or API/token section.
3. Copy your token and paste it in Upify.

### 1fichier (`1fichier.com`)
- Option: `1fichier API key`
- Required: No
- Useful to link uploads to your account/API limits.

How to get it:
1. Sign in to your 1fichier account.
2. Open your API/account settings.
3. Generate/copy API key and paste it in Upify.

### Rootz (`rootz.so`)
- Option: `Rootz folderId`
- Required: No
- Useful if you want uploads in a specific folder.

How to get folderId:
1. Open the target folder on Rootz.
2. Copy the folder ID from URL or folder details.
3. Paste it in Upify.

### Send.now (`send.now`)
- Option: `Send.now recipient`
- Required: No
- This is not an API key, only an optional recipient email behavior.

### Buzzheavier (`buzzheavier.com`)
- Option: `Buzzheavier locationId`
- Required: No
- Default value works for normal usage.

### Vikingfile (`vikingfile.com`)
- Option: `Vikingfile user`
- Required: No
- Optional advanced value depending on your own workflow.

### FileMirage (`filemirage.com`)
- Option: `FileMirage API token`
- Required: No
- Useful if anonymous flow is limited or if your account requires authenticated upload initialization.

How to get it:
1. Sign in to FileMirage.
2. Open API or account token section.
3. Create/copy token and paste it in Upify.

### Pixeldrain (`pixeldrain.com`)
- Option: `Pixeldrain API key`
- Required: **Yes** for Upify API upload.

How to get it:
1. Sign in to Pixeldrain.
2. Open `https://pixeldrain.com/user/api_keys`.
3. Create/copy an API key and paste it in Upify.

### BowFile (`bowfile.com`)
- Options: `BowFile Key 1`, `BowFile Key 2`, `BowFile folder ID` (optional)
- Required: **Yes** (both keys are mandatory; the folder ID is optional)
- Uploads use `/api/v2/authorize` to generate an access token before `POST /api/v2/file/upload`.

How to get the keys:
1. Log in to BowFile.
2. Visit your account dashboard or the API/key management section.
3. Copy **Key 1** and **Key 2** into the corresponding Upify fields.
4. Set a `folder_id` if you want files stored in a specific folder; otherwise leave blank for the default target.

### AkiraBox (`akirabox.to`)
- Option: `AkiraBox API key`
- Required: **Yes**, and the account must be Premium.
- The old `akirabox.com/api/files/upload` endpoint is gone; Upify now uses the
  signed-upload API documented at <https://akirabox.to/developers>:
  `POST /api/upload/start` → `GET /api/upload/chunk-url` → `PUT {signedUrl}` →
  `POST /api/upload/complete`.
- `akirabox.com` redirects to `akirabox.to`; both host names are accepted by Upify.

How to get the key:
1. Sign in at <https://akirabox.to> on a Premium account.
2. Open the account/API settings section.
3. Create an API key and paste it in Upify.

Uploads with two-factor authentication enabled also accept a one-time code
through the local API (`"otp": "123456"`); the web UI does not expose this field.

### 0807.st (`0807.st`)
- Options: `0807.st partner token`, `0807.st expiry in hours`, `0807.st password`
- Required: No. Anonymous uploads work, Upify solves the required proof-of-work
  (SHA-256, leading zero bits) automatically.
- Files up to 2 GB use a single POST (`POST /upload`). Larger files switch to the
  chunked flow (`POST /upload/init` → `POST /upload/chunk` → `POST /upload/finish`).
- `expiry` is a number of hours (`0` never, `1`, `24`, `168`, `720`).
- `password` protects the link.

Optional partner token:
1. Ask 0807.st for a partner token.
2. Paste it in Upify. A partner token skips the proof-of-work, raises the upload
   limit (50 GB single POST, 100 GB chunked) and unlocks custom links.

API reference: <https://0807.st/api>

### DropDrive (`dropdrive.rip`)
- Option: `DropDrive account token`
- Required: No. Anonymous uploads work, Upify solves the required proof-of-work
  (SHA-256, leading zero bits) automatically.
- Uploads are chunked: `GET /api/config`, optional
  `GET /api/upload/pow-challenge`, `POST /api/upload/init`, then every part is
  sent to the assigned storage server, and `POST /api/upload/complete` returns
  the share link.
- With an account token, the proof-of-work is skipped and files are stored in
  that account.

How to get the token:
1. Sign in at <https://dropdrive.rip> (account key/token, Discord, or backup key).
2. Open the account menu and copy the account/backup key.
3. Paste it in Upify.

API reference: <https://dropdrive.rip/docs>

## Removed Hosts

- `ranoz.gg`: the domain no longer resolves (NXDOMAIN) and the service is gone,
  so it was removed from Upify in 1.1.0.

## Security Notes

- Keep API keys private.
- Do not share screenshots showing your keys.
- Revoke/rotate keys if you think they were exposed.
