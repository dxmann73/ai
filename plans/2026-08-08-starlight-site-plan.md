# Starlight Site Plan

Date: 2026-08-08
Topic: Publish this knowledge base as a Starlight docs site at `<domain>/ai`, linked from the website

The notes stay plain Markdown at the repository root. A thin Astro/Starlight build lives in `site/`
and reads them in place. The website at `../website` keeps the blog and links to `/ai` from its main
navigation; the two are separate builds and separate Cloudflare Workers behind one origin.

## Shape

```text
ai/
  communication/ aglc/ business/ society/ adoption/ tools/ reference/   notes, unchanged
  #incoming/ #log/                                                      excluded from the build
  plans/                                                                this file
  site/                                                                 Starlight, package.json, wrangler
```

Routing is path-based: `<domain>/ai/*` resolves to the `site/` Worker, everything else to the
website Worker. Same origin, so the Speculation Rules API can prerender the AI site from the website
and the cross-document navigation reads as instant. The apex domain is not yet decided; until it is,
the site deploys to `*.workers.dev` and every occurrence of the domain lives behind one named
constant per repository.

## Phase 0 — Preconditions

1. Amend `CLAUDE.md` in this repository. The current text says "no build, no framework, no package
   manager". Scope it: notes stay plain Markdown, build tooling is confined to `site/`, and the
   intake and posting workflows are unaffected.
2. Run the GitHub health check on `withastro/starlight` — contributors, recent activity, commit
   frequency, open issues, latest release, license — and report before installing.
3. Confirm the website's live deployment target. `wrangler.jsonc` and a `deploy:cf-workers` script
   point at Cloudflare Workers static assets; `netlify.toml` is stale (it calls `yarn build` while
   the repository uses pnpm). Delete `netlify.toml` once Cloudflare is confirmed as the only target,
   since path routing depends on it.

## Phase 1 — Spike

Goal: prove Starlight renders the notes in place, unmodified. Throwaway work; nothing outside
`site/` changes.

1. Scaffold Starlight into a temporary directory, then move the contents into `site/`.
2. Confirm the current content-layer loader API against the Starlight documentation before writing
   config — whether to use `docsLoader()` from `@astrojs/starlight/loaders` or a plain `glob()`
   loader with `docsSchema()`.
3. Point the docs collection at the repository root, two levels up from `site/src`:
   - base `../..`
   - include `**/*.md`
   - exclude `#*/**`, `site/**`, `node_modules/**`, `plans/**`

   The `#`-prefixed folders must be excluded. `#` is the URL fragment delimiter, so those routes
   cannot work.
4. The notes carry no frontmatter at all — `adoption/beads-adoption.md` and every `README.md` open
   directly with an H1. Starlight requires `title`. Write a loader wrapper that lifts the first H1
   into `title` and strips it from the body so the heading is not rendered twice. Throw on a file
   with no H1; do not fall back to the filename.
5. Relative links between notes point at `.md` files (`../tools/beads-where-issue-data-lives.md`).
   Rewrite them to extensionless routes under the base path.
6. Run `pnpm dev` and click through: sidebar, inter-note links, the `#`-folder exclusions.

Gate: if the H1 lift or the link rewriting turns ugly, add `title:` frontmatter to the notes instead
and drop the wrapper.

## Phase 2 — Site configuration

1. `site/astro.config.mjs`: `base: '/ai'`, `trailingSlash: 'never'` to match the website, `site` set
   from the domain constant.
2. Explicit `sidebar` config in intended order — adoption, aglc, business, communication, society,
   tools, reference — rather than alphabetical autogeneration.
3. Landing page from the root `README.md`. Its `%23incoming/` table links are GitHub-specific, so
   the landing page gets its own navigation.
4. English only. Starlight i18n stays off; the notes are English and the website's DE routes link to
   the same English site.
5. `.gitignore`: add `site/node_modules`, `site/dist`, `site/.astro`.
6. `.markdownlintignore`: add `site/`.

## Phase 3 — Visual continuity

1. Copy the markup of `../website/src/layouts/partials/Header.astro` and the relevant parts of
   `../website/src/styles/navigation.css` into `site/src/components/SiteHeader.astro`.
2. Wire it through `starlight({ components: { Header: './src/components/SiteHeader.astro' } })`.
3. Match fonts from `../website/src/config/theme.json` and the accent colour through Starlight's CSS
   custom properties. Do not import Tailwind or the website's theme config — Starlight has its own
   token system and fighting it costs more than a copied header.
4. Add a link back to the website in the Starlight header.
5. Compare both headers side by side before deploying. Identical headers are what makes the
   cross-document navigation read as one site.

## Phase 4 — Website changes

The AI content and its routes are already removed from the website: the `ai` collection, the
`src/content/ai/**` entries, `src/pages/ai/[slug].astro`, `src/pages/de/ai/[slug].astro`, the `ai.*`
UI keys, and the `isAiContext` sub-navigation branch. What remains is pointing the navigation at the
new site.

1. `src/layouts/partials/Header.astro` — the `AI_SITE_URL` constant currently holds
   `https://ai.example.com` behind a TODO. Set it to `/ai` and drop the TODO comment.
2. Same file: the menu builder passes non-absolute paths through `localeHref(lang, path)`, which
   would turn `/ai` into `/de/ai` under the German locale. That route does not exist. Replace the
   `/^https?:\/\//` test with an explicit per-entry flag that skips locale prefixing, and set it on
   the AI entry.
3. `src/config/menu.json` — the `AI` entry holds the same placeholder. The header builds its main
   navigation from the UI dictionary and does not read this file for it, so the two are duplicated
   sources. Resolve it: either point `menu.json` at `/ai` as well, or remove the main-menu duplicate
   once its remaining consumers are confirmed. Check consumers before deleting.
4. Add the Speculation Rules block to the website so the AI site prerenders on hover:

   ```html
   <script type="speculationrules">
     {"prerender":[{"where":{"href_matches":"/ai/*"},"eagerness":"moderate"}]}
   </script>
   ```

   Mirror it in the Starlight header for links back to the website.
5. The website's `@astrojs/sitemap` output stops at its own routes. Reference the AI site's sitemap
   from `robots.txt` so both are discoverable.
6. Check the remaining `nav.ai` UI key and the tests under `src/i18n/` still pass — the parity test
   and the translation-gap report both walked the `ai` collection before it was removed.
7. `src/config/config.json` still carries `base_url: "https://hydrogen-astro.vercel.app"`. Set it
   when the domain is decided; the AI site's `site` value changes in the same step.

## Phase 5 — Deploy

1. `site/wrangler.jsonc`: own Worker name, `assets.directory: "./dist"`.
2. Deploy to `*.workers.dev` and verify the whole site there first.
3. Once the domain exists, add the route `<domain>/ai/*` to the AI Worker. The website Worker keeps
   everything else.
4. After attaching the route, walk `/`, `/de/`, `/posts`, `/ai`, and `/ai/adoption/beads-adoption` in
   that order and confirm no pattern shadows the main site.
5. Measure the website → `/ai` navigation in DevTools with prerendering active.

## Phase 6 — Workflow

1. Update `CLAUDE.md` here: `site/` exists, every note keeps an H1 as its first line, `#incoming/`
   and `#log/` never publish.
2. Update `README.md` here to name the published URL alongside the read-on-GitHub framing.
3. The posting workflow is unchanged — posts are still composed from the `#log/` pool and written
   into `../website/src/content/posts/en/`. Add one convention: a published post links to the notes
   it draws on at `/ai`, and those notes link back to the post.
4. Add a CI workflow that builds `site/` on push so broken links and missing H1s fail early.

## Accepted costs

Two builds and two deploys. A hand-maintained copy of the header. Two design systems held in sync by
convention rather than by shared code.
