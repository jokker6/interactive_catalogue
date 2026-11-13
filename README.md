# Parts Catalog (prototype)

A static page: the scanned body in the middle, the parts catalog on the side. Hovering a part enlarges it and shows its name, clicking opens its details. STEP files are read directly in the browser, no conversion needed.

## Files

```
index.html      the page (Three.js and the STEP reader load from a CDN)
parts.json      the catalog: one entry per part
scan.glb        the simplified body scan (Blender export)
parts/          STEP (or GLB) files of the parts
```

## Publishing (GitHub Pages)

1. Upload `index.html`, `parts.json`, `scan.glb` and the `parts/` folder to the repository root.
2. **Settings -> Pages**: Source = *Deploy from a branch*, Branch = `main`, folder `/ (root)`.
3. After a few minutes the page is at `https://USERNAME.github.io/REPOSITORY/`.

Pages is public, and a private repository needs a paid GitHub plan to use it.

## Adding a part

The page only shows parts that are listed in `parts.json`. It does not scan the `parts/` folder. So adding a part is always two steps:

1. Upload the STEP file to `parts/` (for example `parts/K6-0001.step`).
2. Add an entry for it to `parts.json`:

```json
{
  "id": "k6-0001",
  "name": "Front bumper",
  "code": "K6-0001",
  "category": "Body front",
  "description": "Short description.",
  "info": { "Material": "Steel", "Price": "120 EUR" },
  "file": "parts/K6-0001.step"
}
```

Remember the comma between entries. File names are case-sensitive on GitHub Pages (`K6-0001.STEP` is not `K6-0001.step`).

If a part cannot be shown, the page lists it in a red box in the top-left corner with the reason (file not found, invalid file, and so on). After uploading, wait a minute or two for Pages to update and refresh with Ctrl+F5.

A part stays where it is in its file, so export parts from the same coordinate system and in millimetres, like the body scan.

If a part does not sit right on the body:

| Field | Meaning |
|---|---|
| `zUp` | `false` if a STEP part comes out lying on its side (the default assumes Z-up) |
| `scale` | scale factor (for example `0.001` if the units are wrong) |
| `position` | offset `[x, y, z]` in body units |

STEP files are converted in the browser when the page opens, which takes a few seconds per part. That is fine for a few dozen parts; for a larger catalog, convert parts to GLB beforehand.

## parts.json fields

| Field | Meaning |
|---|---|
| `id` | unique identifier |
| `name` | name shown on hover |
| `code` | part code |
| `category` | group in the sidebar |
| `description` | text on the info card |
| `info` | any name/value pairs (material, price, ...) |
| `file` | path to the part's .step or .glb |
| `demo` | temporary placeholder shape (used until `file` exists) |
