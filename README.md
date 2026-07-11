# FOIA Hash Verify

**A free, open-source, offline tool to verify that a declassified or FOIA-released document is authentic and unaltered — using its SHA-256 fingerprint.**

Primary-source government records are copied, re-scanned, mislabeled, and occasionally
altered as they spread across the web. This single-file tool lets anyone confirm that a
downloaded PDF is **byte-for-byte identical** to a trusted, published copy — in about ten
seconds, without trusting the host.

👉 **[Try it live](https://verifoia.com/verify)** · 📖 **[Read the full guide](https://verifoia.com/how-to-verify-declassified-documents)**

---

## Why this exists

A SHA-256 hash is a short cryptographic fingerprint of a file's exact bytes. Three properties
make it ideal for document provenance:

- **Deterministic** — the same file always produces the same 64-character fingerprint.
- **Tamper-evident** — change a single byte and the fingerprint changes completely.
- **One-way & private** — the fingerprint reveals nothing about the contents, so it can be
  published openly and checked locally.

If the fingerprint of your copy matches the fingerprint an archive published, you have proof
the file was not altered in transit or re-hosting.

## Features

- 🔒 **100% offline / client-side.** Files are hashed in your browser with the
  [Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto/digest).
  **Nothing is ever uploaded.**
- 🧩 **Zero dependencies, single file.** One `index.html` — no build step, no framework, no CDN.
- 🖱️ **Drag & drop** or click to browse.
- ✅ **Compare mode** — paste the expected hash and get an instant match / no-match verdict.
- 🖥️ **Works from `file://`** — download it and run it with no internet connection at all.

## Use it

**Option A — online:** open [verifoia.com/verify](https://verifoia.com/verify).

**Option B — fully offline:** download `index.html`, open it in any browser, done. Because all
hashing happens locally, running it offline is the most trustworthy option — you can inspect
the source first and confirm it makes no network calls.

**Option C — command line** (no tool needed at all):

```bash
# macOS / Linux
shasum -a 256 document.pdf
sha256sum document.pdf

# Windows
certutil -hashfile document.pdf SHA256
```

Compare the 64-character output against the published hash. Identical = authentic.

## What a matching hash does and doesn't prove

A match proves **integrity** — your copy is identical to the copy that was published and
sealed. It does **not**, on its own, prove **origin** (that a particular agency authored the
record). To establish origin, trace the document back to its official source, such as the
U.S. National Archives. Integrity and provenance reinforce each other.

## How it works (the whole thing)

```js
const buf    = await file.arrayBuffer();
const digest = await crypto.subtle.digest('SHA-256', buf);
const hex    = [...new Uint8Array(digest)].map(b => b.toString(16).padStart(2, '0')).join('');
```

That's it. The rest is UI. Read [`index.html`](index.html) — it's short and unminified on purpose.

## Fork it for your own archive

This tool is intentionally generic. Replace the two `verifoia.com` links in the footer of
`index.html` with your own archive and ship it. Attribution is appreciated but not required
(MIT).

## License

[MIT](LICENSE) — do anything you like, no warranty.

---

Built for the [VeriFOIA](https://verifoia.com/) archive — a cryptographically verified
collection of declassified U.S. government documents, each sealed with a SHA-256 fingerprint.
