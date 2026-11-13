# Parts Catalog (password protected)

A static page: the scanned body in the middle, the parts catalog on the side. Hovering a part enlarges it and shows its name, clicking opens its details (part code, info and a picture of the part). STEP files are read directly in the browser.

## How the password protection works

The page itself (`index.html`) contains no data and no password. The body scan, the part list and all part files are stored **encrypted** (`*.enc`). When someone opens the page it asks for the password and decrypts the data in the browser. Without the password the files are unreadable, even if someone downloads them.

You keep the original (unencrypted) files on your own computer or in Dropbox, and only upload the encrypted versions.

## Files

Your working folder (not uploaded):

```
scan.glb        simplified body scan (Blender export)
parts.json      the catalog: one entry per part
parts/          STEP files of the parts (bumper.step, fender.step, ...)
images/         optional pictures of parts
encrypt.html    the encryption tool (open it in a browser, keep it local)
```

Uploaded to GitHub (repository root):

```
index.html
scan.glb.enc
parts.json.enc
parts/bumper.step.enc, parts/fender.step.enc, ...
images/...enc   (only if you use images)
```

## Updating the catalog

1. Edit `parts.json` and/or add STEP files to `parts/` in your working folder.
2. Open `encrypt.html` in a browser, enter the password, pick your working folder and press **Encrypt and download ZIP**.
3. Unzip, and upload the `.enc` files to the repository (same folders, replacing the old ones).
4. Wait a minute or two for GitHub Pages to update and refresh the page with Ctrl+F5.

Use the same password every time. Never upload the unencrypted `scan.glb`, `parts.json` or `.step` files. If they were uploaded earlier, they stay readable in the repository's history, so the cleanest way is to create a new repository for the protected version and upload only the encrypted files there.

## Adding a part

The page only shows parts that are listed in `parts.json`; it does not scan the `parts/` folder. Add the STEP file to `parts/` and an entry to `parts.json`:

```json
{
  "id": "k6-0007",
  "name": "Front bumper",
  "code": "K6-0007",
  "category": "Body",
  "description": "Short description.",
  "info": { "Material": "Steel", "Price": "120 EUR" },
  "file": "parts/front_bumper.step"
}
```

Remember the comma between entries. File names are case-sensitive on GitHub Pages. If a part cannot be shown, the page lists it in a red box in the top-left corner with the reason.

A part stays where it is in its file, so export parts from the same coordinate system and in millimetres, like the body scan.

## Part picture

When you click a part, a picture of it appears at the bottom of the info card. By default the page renders it from the 3D part itself. To use your own picture instead, add `"image": "images/bumper.png"` to the part's entry (and put the file in `images/`).

## parts.json fields

| Field | Meaning |
|---|---|
| `id` | unique identifier |
| `name` | name shown on hover |
| `code` | part code (shown as "Part code" in the info) |
| `category` | group in the sidebar |
| `description` | text on the info card |
| `info` | any name/value pairs (material, price, ...) |
| `file` | path to the part's .step or .glb |
| `image` | optional picture of the part |
| `zUp` | `false` if a STEP part comes out lying on its side |
| `scale` | scale factor (for example `0.001` if the units are wrong) |
| `position` | offset `[x, y, z]` in body units |

## Password strength

The encrypted files are public, so someone could try to guess the password on their own computer. A short password is fine for testing, but for real use choose a long phrase of 12 or more characters.

## Testing without a password

In `index.html` the line `const REQUIRE_PASSWORD = true;` can be set to `false` to read the plain files directly (use it only on your own computer with a local server, never on the published page).
