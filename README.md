# Vellum — a database-free encrypted blog

A beautiful, static, single-file blog platform. No database, backend, build step, or runtime dependency is needed.

## Features

- Responsive editorial-style reading experience with featured-story slider.
- Post editor with drafts, published stories, categories, tags, cover images, excerpts, and reading-time estimates.
- Search, category filters, bookmarks, shareable story links, and pagination.
- Warm paper, midnight, and sepia themes; configurable accent colour, site identity, and custom hero background image.
- AES-256-GCM encrypted JSON vault using PBKDF2-SHA-256 (310,000 iterations).
- Encrypted backup and restore; encrypted local browser cache.
- Optional direct publishing of the encrypted vault to GitHub.
- GitHub Pages workflow included.

## First-time setup

1. Open the site over HTTPS or on localhost (Web Crypto requires a secure context).
2. On first run, choose a reader passphrase and a separate editor passphrase. Use strong, unique passphrases.
3. Add stories using **Writer's desk**. The reader passphrase is used to encrypt all blog data.
4. Adjust the site title, introduction, theme, accent colour, and background image under **Appearance**.
5. Use **Download encrypted backup** regularly.
6. To publish the encrypted data, create a fine-grained GitHub personal access token limited to this repository with **Contents: Read and write**. In the Writer's desk, enter the repository, branch, token and vault path (`data/vault.json`), then select **Publish encrypted vault**. The token is kept in page memory only and cleared after publishing.

## GitHub Pages

The `.github/workflows/pages.yml` workflow deploys `index.html` and the optional `data/vault.json` file whenever `main` changes. In the repository, open **Settings → Pages** and set **Build and deployment → Source** to **GitHub Actions**. The first deployment may need this one-time setting.

After publishing a vault from the editor, GitHub Actions will redeploy the site and readers can unlock the latest encrypted vault using the reader passphrase.

## Encryption and privacy model

The vault contains only encrypted JSON. AES-256-GCM encrypts the data; PBKDF2-SHA-256 derives the key from the reader passphrase. The passphrase is never stored in the vault. The browser stores only the encrypted vault in local storage.

This is a **password-protected blog**, not a public SEO blog. Readers must enter the blog passphrase to decrypt and read the stories. Anyone who knows that passphrase can read the journal. The separate editor passphrase only gates the writing interface in the browser; it is not server-enforced authorization and should not be treated as multi-user access control. The static site cannot provide server-side permissions or comments.

Cover images added by URL are displayed as external images and are not encrypted within the vault. Do not use sensitive image URLs. For stronger privacy, use non-sensitive images or remove the image URLs.

## Local testing

Run `python3 -m http.server 8000` from the repository directory and open http://localhost:8000.

## License

MIT. See [LICENSE](LICENSE).
