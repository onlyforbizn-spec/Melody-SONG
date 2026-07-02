# Melody Song — Audio Service (Railway)

Node/Express service for the Melody Song (UK) funnel. Trims previews, uploads
audio/lyrics/PDF to the Shopify Files CDN, and exposes polling endpoints.

Adapted from the Mélodine service — identical logic, Melody Song branding on the
lyrics PDF, and its own Shopify credentials.

## Required environment variables (set in Railway)
| Variable | Value |
|---|---|
| `SHOPIFY_SHOP` | `melody-song.com` (or the `xxxx.myshopify.com` domain) |
| `SHOPIFY_CLIENT_ID` | Client ID of the custom app on the Melody Song store |
| `SHOPIFY_CLIENT_SECRET` | Client secret of that same app |
| `PORT` | Optional — Railway sets it automatically |

The Shopify app must have Files read/write scopes (`write_files`, `read_files`).

## Endpoints
- `POST /trim` (multipart, field `data`) → returns trimmed 60s preview mp3
- `POST /save_preview` (multipart `data` + `lead_id`) → uploads `preview_{lead}.mp3`, returns CDN url
- `POST /save_audio` (multipart `data` + `lead_id`) → uploads `{lead}.mp3`
- `POST /save_lyrics` (json `lead_id`, `lyrics`) → uploads `{lead}.txt`
- `POST /save_pdf` (json `lead_id`, `recipient_name`, `lyrics`) → generates + uploads `{lead}.pdf`
- `GET /ready?lead_id=` → `{ready, url}` for the preview
- `GET /full?lead_id=` → `{ready, url}` for the full song
- `GET /lyrics?lead_id=` → `{ready, url}` for the txt
- `GET /pdf?lead_id=` → `{ready, url}` for the pdf
