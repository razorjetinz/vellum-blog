# Vellum — a database-free encrypted blog

Vellum is a static, responsive blog platform. It has no backend, database, build step, or runtime dependencies. Posts, themes, settings, and image data are stored in one encrypted JSON vault.

## Features

- Responsive editorial-style blog layout with a featured story slider.
- Post editor with drafts, categories, tags, cover images, Markdown, reading time, and reading view.
- Search, category filters, sorting, pagination, bookmarks, and sharing.
- Custom hero backgrounds, about image, accent colour, and warm-paper / midnight / sepia themes.
- AES-256-GCM encrypted JSON vault, encrypted backup export, and restore.
- Optional direct publishing of the encrypted vault to GitHub.
- GitHub Pages deployment workflow.
- No cookies, tracking scripts, database, or app server.

## Encryption model and important limitations

Vellum is a **password-protected journal**, not a public SEO blog. Readers enter the blog passphrase to decrypt and read content. The editor passphrase is separate and unlocks the writing desk. The browser derives an AES-256-GCM key using PBKDF2-SHA-256; passphrases are never written into the vault or sent to a server.

A static website cannot keep content both encrypted and publicly readable without distributing a decryption key. Anyone who knows the blog passphrase can read the content. The editor passphrase is a client-side writing-desk gate, not server-enforced authorization. The GitHub publishing token controls repository writes; keep it private and restrict it to this repository.

The browser caches only the encrypted vault in local storage. Decrypted content and passphrases stay in memory while the page is open. Download encrypted backups regularly. If the blog passphrase is lost, the encrypted content cannot be recovered. Cover images are embedded in the encrypted JSON, so keep them reasonably small.

## First-time setup

1. Host the site over HTTPS (GitHub Pages is supported) or on localhost. Web Crypto requires a secure context.
2. On first open, create a blog passphrase for readers and a different editor passphrase.
3. Open **Writer's desk** and add stories. Markdown is supported in story bodies.
4. In **Appearance**, set the title, colours, background images, home-page copy, and slider interval.
5. In **Publish & backup**, download a vault backup or publish `data/vault.json` directly to GitHub.

## GitHub Pages deployment

1. Push the project files to the `main` branch.
2. In GitHub, open **Settings → Pages** and set the source to **GitHub Actions**.
3. The included `.github/workflows/pages.yml` workflow deploys the site when you push to `main`.
4. Open the deployed site and complete first-time setup.
5. In Writer's desk → Publish & backup, enter the repository owner, name, branch, and vault path (`data/vault.json`).
6. Create a fine-grained personal access token restricted to this repository with **Contents: Read and write**. Paste it into the token field only when publishing. The app does not store the token.

## Local testing

Run `python3 -m http.server 8000` in the project directory, then open http://localhost:8000. Do not use a `file://` URL because browser Web Crypto and fetch require a secure context.

## Limitations

There is no server-side comments system, newsletter delivery, analytics, or multi-user collaboration. Posts are rendered after readers unlock the vault, so article text is not indexed by search engines. Anyone with the blog passphrase can read the journal.

## License

MIT. See [LICENSE](LICENSE).
