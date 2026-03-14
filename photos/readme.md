# /photos

Drop your portfolio images here. The carousel reads from this folder.

## Naming Convention
Name your files exactly like this so the carousel picks them up automatically:

```
look-01.jpg   → "MONOLITH" — All-Black Editorial
look-02.jpg   → "MIDNIGHT ARC" — Evening Wear
look-03.jpg   → "STRUCTURE" — Tailored Power
look-04.jpg   → "GROUND ZERO" — Streetwear Direction
look-05.jpg   → "QUIET LOUD" — Brand Identity
```

## Specs
- Format: JPG or WebP (WebP preferred for speed)
- Aspect ratio: 3:4 portrait (e.g. 900×1200px)
- Max file size: 500KB each (compress at squoosh.app)
- Min resolution: 900px wide

## To update captions/titles
Open `index.html` and find the `<!-- CAROUSEL CARDS -->` comment.
Each card has a `data-title` and `data-sub` you can edit directly.

## Adding more photos
Duplicate any `.fan-card` block in `index.html`, increment the index,
add the new image filename, and update the dot count in JS (`const TOTAL`).
