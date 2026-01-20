# UniPaws Pet Landing Page

This is the landing page for pet QR codes. It's designed to be deployed to Cloudflare Pages at `pet.hostyourlab.com`.

## How It Works

1. User scans a pet's QR code (e.g., `https://pet.hostyourlab.com/ABC-1234`)
2. The landing page loads and:
   - Attempts to open the UniPaws app via deep link (`unipaws://pet/ABC-1234`)
   - Fetches pet info from Supabase and displays it
   - Shows app download buttons if the app didn't open
3. If the pet is marked as missing, a red alert banner is displayed

## Setup

### 1. Update Supabase Configuration

In `index.html`, replace these values:

```javascript
const SUPABASE_URL = 'YOUR_SUPABASE_URL';
const SUPABASE_ANON_KEY = 'YOUR_SUPABASE_ANON_KEY';
```

With your actual Supabase project URL and anonymous key.

### 2. Update App Store Links

Replace the placeholder app store URLs:

```javascript
const APP_LINKS = {
    ios: 'https://apps.apple.com/app/unipaws/id000000000',
    android: 'https://play.google.com/store/apps/details?id=com.petrescue.greece'
};
```

### 3. Deploy to Cloudflare Pages

1. Create a new Cloudflare Pages project
2. Connect this directory or upload the `index.html` file
3. Set the custom domain to `pet.hostyourlab.com`
4. Enable "Single-page application" mode (all routes serve index.html)

### 4. Configure Routing (Cloudflare)

Add a `_redirects` file or configure routing so all paths serve `index.html`:

```
/*    /index.html   200
```

Or in `wrangler.toml`:

```toml
[[rules]]
type = "Asset"
globs = ["**/*"]
[[rules]]
type = "Module"
globs = ["_worker.js"]
```

## Features

- **Bilingual Support**: English and Greek translations
- **Missing Pet Alert**: Red banner with details when pet is missing
- **Deep Linking**: Attempts to open the UniPaws app automatically
- **Pet Info Display**: Shows pet photo, name, species, and owner
- **Scan Tracking**: Records web scans for analytics
- **Responsive Design**: Works on all device sizes

## URL Format

The landing page expects URLs in this format:
- `https://pet.hostyourlab.com/ABC-1234` - Pet public ID
- The ID format is 3 uppercase letters + hyphen + 4 digits

## Security Notes

- Only uses Supabase anonymous key (safe to expose)
- Only accesses the `public_pet_info` view (limited data)
- Scan recording uses anonymous insert (no auth required)
