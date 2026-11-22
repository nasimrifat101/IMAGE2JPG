# Image to JPG Converter

A single-file, fully offline image converter. Drop in any image your browser can open and get JPG files back. No server, no uploads, no dependencies, no build step.

## Quick start

1. Download `image-to-jpg.html`.
2. Double-click it to open in Chrome, Edge, Firefox, or Safari.
3. Drop images in, pick them with the file picker, or paste with `Ctrl+V` / `Cmd+V`.
4. Download files one by one, or use **Download all (ZIP)**.

It works with the internet turned off. Everything runs locally in your browser tab.

## Features

- Batch conversion of many files at once
- Quality slider (10–100%, default 90)
- Optional max width/height resize (keeps aspect ratio)
- Background color for transparent areas (default white)
- EXIF orientation fix, so phone photos are not rotated wrong
- Before/after file size per image and in total
- ZIP download built in the browser (no library, no compression since JPGs are already compressed)
- Duplicate filenames get `-2`, `-3`, and so on
- Light and dark mode follow the system setting
- Settings changes re-convert the whole list automatically

## Supported input formats

Decoding is done by the browser, so support depends on the browser.

| Format | Chrome / Edge | Firefox | Safari |
| --- | --- | --- | --- |
| PNG, JPEG, GIF, BMP, ICO | Yes | Yes | Yes |
| WebP | Yes | Yes | Yes |
| AVIF | Yes | Yes | Yes (recent versions) |
| SVG | Yes | Yes | Yes |
| HEIC / HEIF | No | No | Yes |
| TIFF | No | No | Yes |
| RAW (CR2, NEF, ARW, DNG) | No | No | No |

If a file cannot be decoded, only that row shows an error. The rest keep converting.

## How it works

```
File -> createImageBitmap (EXIF-aware) -> fallback to <img> for SVG
     -> resize if max size is set
     -> canvas filled with background color, image drawn on top
     -> canvas.toBlob('image/jpeg', quality)
     -> download as .jpg or add to ZIP
```

## Limits and edge cases

- **Transparency:** JPG has no alpha channel. Transparent pixels are filled with the chosen background color.
- **Animated GIF/WebP:** only the first frame is converted.
- **Very large images:** anything above 100 megapixels is rejected to avoid crashing the tab. Some phones and browsers fail earlier because of canvas memory limits.
- **SVG without a width/height:** falls back to 1024×1024. Set a max size if you need something else.
- **Color profiles:** output is standard sRGB. Wide-gamut colors (Display P3, Adobe RGB) may look slightly different.
- **Metadata:** EXIF, GPS, and other metadata are stripped. This is good for privacy, but it also means camera info is lost.
- **Re-encoding:** converting JPG to JPG re-compresses it and loses a little quality. Use quality 100 if you need to keep it close.

## Privacy and security

- No network requests. The page loads nothing from the internet.
- Files are never uploaded or stored. Everything stays in memory and is released when you remove an item or close the tab.
- File names are written to the page with `textContent`, so a malicious filename cannot inject HTML.
- Output file names have unsafe characters (`\ / : * ? " < > |`) replaced with `_`.

## Troubleshooting

| Problem | Fix |
| --- | --- |
| HEIC or TIFF fails | Open the file in Safari, or convert it with a desktop tool first. |
| "Image is too large" | Set a max width/height (for example 4000) and try again. |
| Download does nothing | Check that the browser is not blocking multiple downloads. Use the ZIP button instead. |
| Colors look different | See the color profile note above. |
| Nothing happens on drop | Drop on the dashed area, or use the click-to-choose option. |

## Possible next steps

- **HEIC / TIFF / RAW in any browser:** a small Node.js CLI using `sharp` (works offline, needs install).
- **Installable offline app (PWA):** add a manifest and service worker, and host it once over HTTPS.
- **Desktop app:** wrap the same file with Tauri or Electron.
- **Web Worker + OffscreenCanvas:** keep the UI smooth when converting hundreds of large images.

## File layout

```
image-to-jpg.html   # the whole app (HTML + CSS + JS)
README.md
```

## License

Use it however you like, for personal or commercial work.
