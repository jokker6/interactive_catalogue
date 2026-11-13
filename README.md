# Parts Catalog (password protected)

A static page: the scanned body in the middle, the parts catalog on the side. Hovering a part enlarges it and shows its name, clicking opens its details (part code, info and a picture of the part). STEP files are read directly in the browser.

## Access protection (current setup: simple password screen)

The page asks for a password before it shows anything. This is a **simple screen, not real protection**: the files (`scan.glb`, `parts.json`, STEP files) are stored as they are, so anyone who knows or guesses their web addresses can still download them. If the GitHub repository is public, anyone can also read them there. It keeps casual visitors out and nothing more.

To change the password: compute the SHA-256 of the new password and put it in `GATE_HASH` near the top of the script in `index.html`. Setting `GATE_HASH` to `''` removes the screen.

For real protection later there are two options:
- **Cloudflare Pages + Cloudflare Access** (free for up to 50 users): visitors log in with their e-mail, the files are not served to anyone else. The repository can stay private.
- **Encrypted files**: set `REQUIRE_PASSWORD = true` in `index.html` and use `encrypt.html` to encrypt the files before uploading them (`scan.glb.enc`, `parts.json.enc`, ...). Then the files are unreadable without the password.

## Files

Uploaded to GitHub (repository root):

```
index.html
scan.glb        simplified body scan (Blender export)
parts.json      the catalog: one entry per part
parts/          STEP files of the parts
images/         optional pictures of parts
```

(`encrypt.html` is only needed for the encrypted setup and is not uploaded.)

## Updating the catalog

1. Edit `parts.json` and/or add STEP files to `parts/`.
2. Upload the changed files to the repository.
3. Wait a minute or two for GitHub Pages to update and refresh with Ctrl+F5.

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
