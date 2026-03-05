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
| `ranoz.gg` | none | No | Not needed in current Upify flow |
| `vikingfile.com` | `Vikingfile user` | No | Optional if you use a specific user/path flow |
| `filemirage.com` | `FileMirage API token` | No | Yes, if your account/server requires auth |
| `pixeldrain.com` | `Pixeldrain API key` | **Yes** | **Required for API upload** |

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

### Ranoz (`ranoz.gg`)
- No API key field in Upify currently.
- Required: No.

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

## Security Notes

- Keep API keys private.
- Do not share screenshots showing your keys.
- Revoke/rotate keys if you think they were exposed.
