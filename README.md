# Fuji Toner Scanner

A small web app for iPhone (or any phone) that scans the barcode on a Fujifilm / Fuji Xerox toner box and shows the part number, colour, page yield and compatible printer models.

Fuji toner boxes carry a **Code 39** barcode next to 型號 (e.g. `*CT202634*`) that encodes the part number directly, so the app just looks that part number up in your list.

## Features

- **🎥 Live**: point the camera at the barcode and it scans automatically.
- **📷 Scan barcode**: take a photo and the app reads the barcode from it (use this if live scanning has trouble).
- Search by part number, printer model or colour.
- Add, edit and delete toners. Scanning an unknown part opens the add form already filled in.
- Your list is saved on the phone (browser storage). **Export list** and **Import list** back it up or move it to another device.
- Can be added to the Home Screen like an app.

There's no server and no account. Everything runs in the browser. Barcode decoding uses [ZXing](https://github.com/zxing-js/library), loaded from jsDelivr.

## Publish on GitHub Pages

1. Create a new repository on GitHub, e.g. `fuji-toner-scanner`.
2. Upload every file in this folder to it, including `.nojekyll`.
3. In the repository, open **Settings → Pages**. Under **Build and deployment**, set Source to **Deploy from a branch**, Branch to **main** and folder to **/ (root)**, then click **Save**.
4. After a minute or so the app is live at `https://<your-username>.github.io/fuji-toner-scanner/`.
5. On your iPhone, open that address in Safari, tap **Share → Add to Home Screen**, and allow camera access when asked.

The camera only works over HTTPS. GitHub Pages provides HTTPS automatically.

## Your toner list

- `toners.json` is the starting list a phone loads **the first time** it opens the app. After that, the phone keeps its own copy.
- To share an updated starting list with new devices, use **Export list**, replace `toners.json` in the repository with the exported file, and commit.
- To copy your current list to another phone, Export on one and Import on the other.

Each entry looks like this:

```json
{
  "part": "CT202634",
  "color": "Black",
  "models": ["DocuCentre-VI C2271", "C3371"],
  "yield": 34000,
  "notes": "Shelf B2"
}
```

`color` is one of: Black, Cyan, Magenta, Yellow, Drum, Waste toner, Other.

The four starter entries (CT202634–CT202637) come from retailer listings. Check them against your printer's supplies list.

## Privacy note

A public GitHub repository makes `toners.json` visible to anyone. Only the starting list is published. Anything you add on your phone stays on your phone.
