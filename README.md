# Stipple

> All the dot variations of one email address. Locally in your browser.

**Stipple** generates every possible permutation of dots inside the local part of an email address (everything before `@`). For Gmail addresses, all of these variations route to the exact same inbox — Gmail simply ignores dots in the local part.

The name comes from *stippling* — the art technique of marking a surface with countless small dots.

## Live demo

Open the GitHub Pages site of this repository (see the link at the top of the repo).

## Features

- Paste any email — Stipple immediately produces every dot variation (e.g. a 9-character local part yields **256** variants, 10 chars → 512, 12 chars → 2048, …).
- **Mark addresses as used** — toggle `Use` on any row; the state persists in `localStorage` (key `stipple_used_v1`) so it survives refreshes and re-generations.
- **Filter** by *all / unused / used* with live counts.
- **Copy** a single address (click the row or the `Copy` button) or copy the entire current list at once.
- **Download** the current list as a plain `.txt` file.
- **Clear marks** wipes the local cache.
- Pagination at 50 rows per page so even tens of thousands of variations stay snappy.
- Works **fully offline** — no servers, no telemetry, no network requests for your data. (Only Google Fonts is loaded over CDN; you can self-host the fonts if you don't want even that.)

## How it works

Given a local part of length `N` (with any existing dots stripped first), there are `N − 1` slots between characters. Each slot may either contain a dot or not, giving `2^(N − 1)` total combinations. Stipple iterates over every bitmask from `0` to `2^(N-1) − 1` and inserts dots accordingly.

```
johnsmith@gmail.com   →   256 variants
                          johnsmith@gmail.com         (mask 00000000)
                          j.ohnsmith@gmail.com        (mask 00000001)
                          jo.hnsmith@gmail.com        (mask 00000010)
                          ...
                          j.o.h.n.s.m.i.t.h@gmail.com (mask 11111111)
```

Every variation in that list is delivered to **the same Gmail inbox** because Gmail (and Googlemail) ignore dots in the local part. For other providers (Yandex, Outlook, Mail.ru, …) the dot is significant — Stipple still generates the same combinatorial set, but those addresses will be different mailboxes.

## Why use it?

- Bypass per-address limits on services that allow only one signup per email but don't normalize Gmail dots.
- Generate distinct "tracking" addresses for newsletters/services to see which one starts leaking your address.
- Educational: visualise just how many "different" Gmail addresses are actually one and the same.

## Tech

- A single static `index.html` — vanilla HTML / CSS / JavaScript, no build step, no dependencies.
- About **27 KB** uncompressed; loads instantly.
- Uses `localStorage` for persistence of "used" marks.
- Geist + Geist Mono via Google Fonts (with system-font fallbacks if offline).

## Local development

There is nothing to build. Just open `index.html` in a browser, or serve the folder with any static server:

```bash
# Python 3
python -m http.server 8080

# Node
npx serve .
```

Then visit <http://localhost:8080>.

## Privacy

Stipple does **not** send your email anywhere. Open DevTools → Network and you'll see only the Google Fonts CSS request. The address you paste, the variations, and the "used" marks all live in your browser's `localStorage` and never leave your machine.

To wipe state: open DevTools → Application → Local Storage → delete the key `stipple_used_v1`, or click the **Очистить отметки** button in the UI.

## Browser support

Modern Chromium, Firefox, Safari, and Edge. Uses `localStorage`, the Clipboard API, and standard ES2018+ JavaScript.

## License

MIT — see `LICENSE`.

## Disclaimer

This is a utility for understanding email semantics and the Gmail dot trick. Don't use it to violate the Terms of Service of any site you sign up for.
