# STUDIO — Stylist Booking Site

Clean static site. No build step. Deploys to Vercel in seconds.

---

## Repo Structure

```
studio-booking/
├── index.html          ← entire site
├── vercel.json         ← Vercel config
├── .gitignore
├── README.md
└── photos/
    ├── README.md       ← photo specs & naming guide
    ├── look-01.jpg     ← drop your photos here
    ├── look-02.jpg
    ├── look-03.jpg
    ├── look-04.jpg
    └── look-05.jpg
```

---

## Deploy

```bash
git init
git add .
git commit -m "init: studio booking site"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/studio-booking.git
git push -u origin main
```

Then: vercel.com → New Project → Import repo → Framework: **Other** → Deploy.

Push to main = auto-deploy. Every time.

---

## 1. EmailJS Setup

EmailJS sends every booking form submission directly to your inbox. Free tier = 200 emails/month.

**Step 1 — Create account**
Go to https://emailjs.com → Sign up free

**Step 2 — Add email service**
Dashboard → Email Services → Add New Service
→ Choose Gmail (or your provider) → Connect your account
→ Copy the **Service ID** (looks like `service_xxxxxxx`)

**Step 3 — Create email template**
Dashboard → Email Templates → Create New Template

Set the template up like this:

```
Subject: New Booking Request — {{service}}

From: {{first_name}} {{last_name}}
Email: {{email}}
Phone: {{phone}}
Service: {{service}}

Message:
{{message}}
```

→ Save → Copy the **Template ID** (looks like `template_xxxxxxx`)

**Step 4 — Get your public key**
Dashboard → Account → API Keys → Copy **Public Key**

**Step 5 — Paste into index.html**
Open `index.html`, find the `CONFIG` block near the bottom:

```javascript
const CONFIG = {
  emailjs_public_key:  'YOUR_EMAILJS_PUBLIC_KEY',   // ← paste here
  emailjs_service_id:  'YOUR_SERVICE_ID',            // ← paste here
  emailjs_template_id: 'YOUR_TEMPLATE_ID',           // ← paste here
  ...
```

---

## 2. Photos Setup

Photos live in the `/photos` folder. The carousel auto-loads them.

**Step 1 — Add your photos**
Drop your files into `/photos/` using this exact naming:
```
look-01.jpg
look-02.jpg
look-03.jpg
look-04.jpg
look-05.jpg
```

Specs: JPG or WebP · 3:4 portrait · 900×1200px min · compress at squoosh.app

**Step 2 — Connect photos to carousel cards**
Open `index.html`. Find each `.fan-card` — they have `data-photo=""`.
Fill in the filename:

```html
<div class="fan-card" data-idx="0" data-photo="look-01.jpg">
<div class="fan-card" data-idx="1" data-photo="look-02.jpg">
<!-- etc -->
```

That's it. JavaScript handles the rest. The gradient placeholder shows
automatically if a photo is missing or not yet added.

**To update a card's title/category:**
Find the card's `.card-title` and `.card-sub` and edit the text directly.

---

## 3. Google Reviews Setup

Pulls your live Google reviews and star rating automatically.

**Step 1 — Get a Google Cloud API key**
1. Go to https://console.cloud.google.com
2. Create a new project (or use existing)
3. APIs & Services → Enable → search "Places API (New)" → Enable
4. APIs & Services → Credentials → Create Credentials → API Key
5. Click the key → Application restrictions: **HTTP referrers**
6. Add your domain: `https://yourdomain.com/*` and `https://*.vercel.app/*`
7. API restrictions: restrict to **Places API (New)** only
8. Copy the key

**Step 2 — Find your Google Place ID**
Go to: https://developers.google.com/maps/documentation/places/web-service/place-id
Use the Place ID Finder tool → search your business name → copy the Place ID
(looks like `ChIJ...` or a long string)

**Step 3 — Get your Google Maps CID link**
Search your business on Google Maps → click your listing → copy the URL from the browser bar
(use this for the "See all reviews" link)

**Step 4 — Paste into index.html CONFIG block**

```javascript
const CONFIG = {
  ...
  google_place_id: 'ChIJYourPlaceIdHere',
  google_api_key:  'AIzaSyYourKeyHere',
  google_maps_url: 'https://maps.google.com/?cid=YourCIDhere'
};
```

**What happens with no key?**
The site shows 6 hand-written placeholder reviews until you add the real key.
It never shows an error to visitors.

---

## Updating the site

```bash
# Make your changes to index.html or drop new photos in /photos/
git add .
git commit -m "update: new photos added"
git push
# Vercel auto-deploys within ~10 seconds
```

---

## Custom Domain

Vercel dashboard → Project → Settings → Domains → Add domain
Add these DNS records at your registrar:
- A record: `@` → `76.76.21.21`
- CNAME: `www` → `cname.vercel-dns.com`
