# Smart City website

This `docs/` folder is the GitHub Pages site for the public Smart City release repository.

Enable Pages in repository settings with **Deploy from a branch**, branch `main`, folder `/docs`.
The site intentionally links to the existing v1.0.0 release assets and does not contain game source, AI models, or private files.

## Local review

From the repository root, run `python -m http.server 8080 --directory docs`, then open `http://localhost:8080/`.

The site is mobile-first, uses repository-relative assets, and requires no build step or server backend.
