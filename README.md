# Toner Stocktake

A phone web app for stocktaking Fujifilm / Fuji Xerox toner. Scan the barcode on each box and that part's count goes up by 1. When you're done, download the counts as an Excel file, or have every scan go live to a Google Sheet.

Fuji toner boxes carry a **Code 39** barcode next to 型號 (e.g. `*CT202634*`) that contains the part number itself.

## How a stocktake works

1. **Settings → Start new stocktake** sets every count to 0. You can download the previous stocktake first.
2. Tap **🎥 Scan (live camera)** and point the phone at each box in turn. Each box adds **+1**, with a beep and a green flash.
   - Holding the same barcode in view doesn't keep counting. Move the camera off it for about 1.5 seconds before scanning the next box of the same part.
   - **📷 Scan from photo** is a backup for barcodes the live camera won't read.
3. The **Last scanned** card shows the part and its count, with **Undo**, **−1**, **+1** and **Edit** buttons.
4. **📋 Stock list** shows every counted part and the total.
5. **⬇︎ Download Excel** saves `Stocktake <date>.xlsx` with two sheets:
   - **Summary:** part, colour, compatible models, quantity counted, plus a total.
   - **Scan log:** every +1, −1 and undo, with the time.

Parts not in your toner list are still counted. Tap **Edit** to add their colour and models.

## Where the results go

Choose under **Settings → Save results to**:

| | Excel file (download) | Google Sheet (live) |
|---|---|---|
| Setup | None | About 5 minutes, once |
| Updates | When you tap Download Excel | With every scan |
| Works offline | Yes | Scans wait on the phone and send when you're back online |
| Excel download | Yes | Yes, still available |

### Setting up the Google Sheet

1. Create a new Google Sheet.
2. Go to **Extensions → Apps Script**, delete the code that's there, and paste in the contents of [`google-apps-script.gs`](google-apps-script.gs). Save.
3. Click **Deploy → New deployment**. Under **Select type**, choose **Web app**. Set **Execute as** to **Me** and **Who has access** to **Anyone**, then click **Deploy** and approve the permissions.
4. Copy the **Web app URL** (it ends in `/exec`), paste it into the app under **Settings → Google Sheet**, and tap **Send test row**. A TEST row should appear in the sheet's **Scan log** tab.

The sheet then has a **Summary** tab with live quantities per part and a **Scan log** tab with every change. Starting a new stocktake clears the Summary tab. The Scan log tab keeps all history.

> Anyone who has the web app URL can add rows to your sheet, so keep the URL private. It can't read your sheet.

## Publish on GitHub Pages

1. Create a new repository on GitHub, e.g. `toner-stocktake`.
2. Upload every file in this folder, including `.nojekyll`. On a Mac, press Cmd+Shift+. in Finder to show hidden files.
3. Go to **Settings → Pages**. Set **Source** to **Deploy from a branch**, the branch to **main** and the folder to **/ (root)**, then click **Save**.
4. Open `https://<your-username>.github.io/toner-stocktake/` in Safari on your iPhone. Tap **Share → Add to Home Screen** and allow camera access when asked.

The camera only works over HTTPS. GitHub Pages provides HTTPS automatically.

## Your toner list

- **Settings → Toner list backup** exports and imports the list. To give new devices an updated starting list, replace `toners.json` in the repository with an exported file.

Barcode reading uses [ZXing](https://github.com/zxing-js/library) and Excel files are made with [SheetJS](https://sheetjs.com). Both load from jsDelivr.
