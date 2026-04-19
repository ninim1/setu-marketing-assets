# SETU Marketing Assets

Permanent public asset hosting for SETU Nutrition's creative pipeline.

## Why this exists

Canva, Replicate, and other creative tools need publicly-fetchable URLs to ingest images. Temporary hosts (transfer.sh, 0x0.st) are now blocking AI traffic. Drive shared links have permission friction. **GitHub raw URLs never expire, are free, and work with every tool.**

This repo is the single source of truth for:
1. **Product renders** — 3D box + sachet/pouch renders (per product)
2. **Generated hero photos** — Nano Banana Pro outputs (text-free versions for Canva ingestion)
3. **Brand assets** — logos, color system JSON, typography specs
4. **Lifestyle photography** — master photoshoot crops

## URL pattern

```
https://raw.githubusercontent.com/ninim1/setu-marketing-assets/main/{path}
```

Example:
```
https://raw.githubusercontent.com/ninim1/setu-marketing-assets/main/renders/creatine-glutone/box.png
```

Use these URLs in:
- Canva `upload-asset-from-url`
- Replicate `image_input` parameter (as alternative to Replicate Files API — persistent)
- Magic Patterns LP imports
- Klaviyo email hero images
- Anywhere else a public URL is needed

## Structure

```
setu-marketing-assets/
├── brand/                      # Logos, color system, typography
├── renders/                    # Product renders (box, sachet, pouch)
│   ├── creatine-glutone/
│   ├── skin-acne-clear-powder/
│   └── electrolytes/
├── photography/                # Master photoshoot crops
│   └── lifestyle/
└── generated/                  # AI-generated hero photos (text-free for Canva)
    └── {product}/
        └── {campaign}/
            └── C1-pilates-textfree.jpg
```

## Workflow

### Adding a new product render
```bash
cp ~/setu-ai-agents/setu-marketing-bot/assets/3d-renders/{product}/box.png \
   ~/setu-marketing-assets/renders/{product}/box.png
cd ~/setu-marketing-assets
git add . && git commit -m "add {product} renders" && git push
```

### Adding a generated hero photo (for Canva ingestion)
```bash
cp ~/setu-ai-agents/setu-marketing-bot/creative-output/{product}/{campaign}/photo.jpg \
   ~/setu-marketing-assets/generated/{product}/{campaign}/photo.jpg
cd ~/setu-marketing-assets
git add . && git commit -m "add {product} {campaign} hero" && git push
```

### Using in Canva
```python
canva.upload_asset_from_url(
    url="https://raw.githubusercontent.com/ninim1/setu-marketing-assets/main/generated/creatine-glutone/icp-deep-v1/C1-pilates-textfree.jpg",
    name="creatine-C1-pilates-hero"
)
```

## Rules

- **Public repo** — required for Canva/etc to fetch without auth. Do NOT put confidential material here.
- **No customer data** — before/after photos only with written consent.
- **Small files only** — use git LFS if files exceed 50 MB.
- **Descriptive filenames** — `{icp}-{stage}-{angle}.jpg` format (e.g., `C1-unaware-pilates.jpg`).

## Owner

Nihaal Mariwala (nihaal@setu.in). For access questions or additions, open an issue.
