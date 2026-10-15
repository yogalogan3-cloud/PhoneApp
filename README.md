# Site Visit Quote

A phone app for landscaping site visits. It turns photos, a recorded client conversation and measurements into an AI design concept and a quotation you can save as a PDF or send on WhatsApp.

## What uses AI

| Feature | Service | Key |
|---|---|---|
| Recording → text (English + BM) | OpenAI `gpt-4o-transcribe` | `OPENAI_API_KEY` |
| Key notes + suggested zones | Claude | `ANTHROPIC_API_KEY` |
| Before/after design concept from your photo | OpenAI `gpt-image-1` | `OPENAI_API_KEY` |
| Plant palette | Claude | `ANTHROPIC_API_KEY` |
| Quotation lines from your rate card | Claude | `ANTHROPIC_API_KEY` |

The keys stay on the Vercel server. They are never sent to the phone.

## Set up (about 15 minutes)

1. **Get an OpenAI key.** Go to platform.openai.com → sign in → Settings → Billing → add RM 20–50 credit. Then API keys → Create new secret key → copy it.
   Image editing needs a verified organisation: Settings → Organization → General → Verify. It takes a few minutes with your IC or passport.
2. **Put the code on GitHub.** Create a new repository (for example `site-visit-quote`) and upload every file in this folder, keeping the `api` folder as it is.
3. **Deploy on Vercel.** vercel.com → Add New → Project → import the repository → Deploy. No build settings are needed.
4. **Add the keys.** In Vercel open the project → Settings → Environment Variables and add:
   - `ANTHROPIC_API_KEY` = your Claude key
   - `OPENAI_API_KEY` = your OpenAI key
   - `APP_PASSCODE` = any code you choose (recommended, so strangers who find the link cannot use your credit)
   
   Then go to Deployments → ⋯ on the latest → **Redeploy** so the keys take effect.
5. **Open it on your phone.** Open the Vercel link in Chrome (Android) or Safari (iPhone). Go to Settings → enter your passcode → tap **Check AI connection**. Both should say *connected*.
   Add it to your home screen: Safari → Share → Add to Home Screen, or Chrome → ⋮ → Add to Home screen.
6. **Put in your own rates.** Settings → Rate card. The starting rates are samples only.

## Rough running cost per site visit

- 15 minutes of recording: about RM 0.30
- Each concept image (medium quality): about RM 0.20–0.35
- Notes, plants and quotation: about RM 0.10–0.20 in total

## Good to know

- Visits, photos and settings are saved **on the phone only**, inside the browser. Clearing browser data deletes them. Sync between phones (for example with Supabase) can come later.
- The microphone and GPS need the app to be opened from the https Vercel link. They do not work if you open `index.html` as a file.
- **Save PDF** opens the phone's print screen. On iPhone, tap Share → Save to Files. On Android, choose Save as PDF.
- **Send on WhatsApp** opens a chat with the client and a ready message. Attach the PDF you saved.
- If concept images time out, set the environment variable `OPENAI_IMAGE_QUALITY` to `low`.

## Optional settings (environment variables)

| Name | Default | Purpose |
|---|---|---|
| `ANTHROPIC_MODEL` | `claude-sonnet-5-5` | Claude model for notes, plants and quotes |
| `OPENAI_IMAGE_MODEL` | `gpt-image-1` | Image editing model |
| `OPENAI_IMAGE_QUALITY` | `medium` | `low`, `medium` or `high` |
| `OPENAI_TRANSCRIBE_MODEL` | `gpt-4o-transcribe` | Speech-to-text model |

## Files

- `index.html`: the whole app (screens, saving, PDF layout)
- `api/transcribe.js`, `api/notes.js`, `api/palette.js`, `api/quote.js`, `api/concept.js`: AI calls
- `api/ping.js`: checks which keys are set
- `vercel.json`: gives AI calls up to 60 seconds
