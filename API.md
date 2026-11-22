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
| `0807.st` | `0807.st partner token` | No | ⚠️ Currently blocked by Cloudflare legal order |
| `dropdrive.rip` | `DropDrive account token` | No | Stores uploads in your account instead of anonymously |
| `new.fileditch.com` | `Fileditch API key` | No | Optional; **required when uploading from cloud/VPS IPs** |
| `mocha.my` | `Mocha API key` | **Yes** | **Required for authenticated uploads** |

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
- **Currently unavailable**: `0807.st` is blocked by a Cloudflare legal order (HTTP 451 / Error 1026). Uploads to this host will fail until the block is lifted.
- Required: No. Anonymous uploads work when accessible, Upify solves the required proof-of-work
  (SHA-256, leading zero bits) automatically.
- Files up to 32 MB use a single POST (`POST /upload`); the direct endpoint
  answers HTTP 413 above that. Larger files use the chunked flow
  (`POST /upload/init` → `POST /upload/chunk` → `POST /upload/finish`), which
  streams each part from disk.
- `expiry` is a number of hours (`0` never, `1`, `24`, `168`, `720`).
- `password` protects the link.

Optional partner token:
1. Ask 0807.st for a partner token.
2. Paste it in Upify. A partner token skips the proof-of-work, raises the upload
   limit (50 GB single POST, 100 GB chunked) and unlocks custom links.

API reference: <https://0807.st/api>

### Fileditch (`new.fileditch.com`)
- Option: `Fileditch API key` (optional)
- Required: No on residential connections; **required on cloud/VPS IPs** (AWS,
  Azure, Google Cloud and similar). Fileditch returns HTTP 401 with
  `code: "api-key-required"` in that case.
- Uploads use `POST`/`PUT https://new.fileditch.com/upload.php?filename=…` with
  the raw file body. Upify sends a streaming `PUT` with `Content-Length`, which
  avoids the `411`/`413` errors of the old `/upload` route.
- The response contains `url`, `filename` and `size`; the API key, when set, is
  sent as `Authorization: Bearer …`.

### Mocha (`mocha.my`)
- Option: `Mocha API key`
- Required: **Yes**. Mocha's upload API is authenticated; anonymous uploads are
  no longer accepted.
- Uploads use the multipart flow: `POST /api/files/multipart/init`, then either
  direct parts (`PUT /api/files/multipart/part`) or S3 presigned parts
  (`POST /api/files/multipart/presigned`), then
  `POST /api/files/multipart/complete`. Upify creates the share link with
  `POST /api/shares` and returns `https://mocha.my/share/<token>`.

How to get the token:
1. Sign in at <https://mocha.my>.
2. Open the account/API settings.
3. Copy your API token and paste it in Upify.

### DropDrive (`dropdrive.rip`)
- Option: `DropDrive account token`
- Required: No. Anonymous uploads work, Upify solves the required proof-of-work
  (SHA-256, leading zero bits) automatically.
- Uploads start with `GET /api/config`, optional
  `GET /api/upload/pow-challenge`, then `POST /api/upload/init`.
- The init response selects the backend:
  - `uploadBackend: "s3"` with `uploadMode: "single"`: Upify `PUT`s the whole
    file to `uploadUrl`, then calls `POST /api/upload/s3-complete`.
  - `uploadBackend: "s3"` with multiple parts: Upify signs each part with
    `POST /api/upload/s3-part`, `PUT`s it to the signed URL and sends all ETags
    to `POST /api/upload/s3-complete`.
  - Otherwise: Upify sends each raw part to the returned `storageServerUrl`.
- Finally `POST /api/upload/complete` returns the share link. S3 sessions send
  their per-session `uploadToken`; storage-server sessions do not.
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
