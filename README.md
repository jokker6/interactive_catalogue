# Detailikataloog (prototüüp)

Staatiline leht: 3D-kere keskel, detailid kataloogina küljel. Hiirega detaili peale minnes see suureneb ja kõrvale tuleb nimi, klõps avab info.

## Failid

```
index.html      leht ise (Three.js tuleb CDN-ist)
parts.json      detailide kataloog (nimi, tootekood, kategooria, info)
scan.glb        lihtsustatud kere (sinu Blenderi eksport)
parts/          detailide .glb failid (valikuline, vt allpool)
```

## Avaldamine GitHub Pagesis

1. Tee GitHubis uus repo (nt `katalog`).
2. Lae repo juurkausta üles `index.html`, `parts.json` ja `scan.glb` (vajadusel kaust `parts/`).
3. Repo **Settings → Pages → Build and deployment**: Source = *Deploy from a branch*, Branch = `main`, kaust `/ (root)`.
4. Mõne minuti pärast on leht aadressil `https://KASUTAJA.github.io/katalog/`.

GitHubi limiit on 100 MB faili kohta, seega kere GLB peab olema sellest väiksem (Draco tihendusega on).

## Kohalik test

Lehte ei saa avada topeltklõpsuga (`file://`), vaja on lihtsat serverit. Kaustas:

```
python3 -m http.server 8000
```

ja ava `http://localhost:8000`.

## Päris detaili lisamine

1. Blenderis impordi detail samasse stseeni kere kõrvale, nii et see on õiges kohas. Ära liiguta kere.
2. Vali ainult see detail ja ekspordi **glTF Binary (.glb)**, märgi **Limit to → Selected Objects**.
3. Pane fail kausta `parts/` (nt `parts/K6-0001.glb`).
4. Lisa `parts.json`-is detaili kirjesse `"file": "parts/K6-0001.glb"` ja eemalda `"demo"` rida.

Detail jääb sinna, kuhu Blenderis jäi, sest glTF säilitab asukoha.

## STEP failide näitamine otse

Lehel on STEP-i lugeja sees (OpenCascade WebAssembly, laetakse alles siis, kui mõni detail on STEP). GLB-d pole vaja teha.

1. Pane STEP-fail kausta `parts/` (nt `parts/K6-0001.step`).
2. Lisa `parts.json`-is kirjesse `"file": "parts/K6-0001.step"` ja eemalda `"demo"` rida.

Detail jääb sinna, kuhu ta STEP-failis on. Seega ekspordi Fusionist detail samast koordinaatsüsteemist, kus kere scan on.

Lisaväljad, kui detail ei istu kere peal:

| Väli | Tähendus |
|---|---|
| `zUp` | `false`, kui detail tuleb külili (vaikimisi eeldab Z-üles STEP-i) |
| `scale` | skaala (nt `0.001`, kui ühikud on valed) |
| `position` | nihe `[x, y, z]` kere ühikutes |

Märkused: STEP-i teisendus brauseris võtab detaili kohta sekundeid ja laadib kõik detailid kohe lehe avamisel, seega sobib see mõne kümne detaili jaoks. Suurema kataloogi puhul tasub detailid hiljem GLB-ks konverteerida.

## parts.json väljad

| Väli | Tähendus |
|---|---|
| `id` | unikaalne tunnus |
| `name` | nimi, mis tuleb hiirega üle sõites |
| `code` | tootekood |
| `category` | grupp külgpaneelis |
| `description` | kirjeldus info kaardil |
| `info` | suvalised nimi-väärtus paarid (materjal, hind jne) |
| `file` | detaili .glb tee |
| `demo` | ajutine kohatäide kere mõõtude järgi (kuni `file` on olemas) |
