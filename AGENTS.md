## Learned User Preferences

- Prefer restrained hero typography on the landing page: keep the H1 and hero intro paragraph at a smaller scale rather than oversized display type.
- For social and SEO metadata, use absolute image URLs that return a real image over HTTP; do not use GitHub blob page URLs as `og:image` sources.

## Learned Workspace Facts

- The public Crawlboy landing site is served at https://crawlboy.aksharahegde.xyz/; canonical, Open Graph, Twitter, JSON-LD `url`, `robots.txt`, and `sitemap.xml` should stay aligned with that origin when it is production.
- The github.io path for this project previously served `og.png` with 404 while the custom domain returned 200; verify preview URLs against the live host rather than assuming github.io matches production.
- The marketing page is a static `index.html` at the repo root with `logo.png` and `og.png` in the root for favicon and social preview assets.
- The nav home link uses a relative base (e.g. `./`) so the site works when hosted under a GitHub Pages project path.
