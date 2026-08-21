# The Sourcing Engine

A static site (plain HTML, no build step, no framework) documenting a West-to-India
product sourcing engine. Deployed on Vercel from the `main` branch of this repo.

## Rules — read before doing anything

**You run the whole loop: research → edit → commit → push → deploy.** Arfa no longer
runs git by hand. Work on the branch you were given (or create it), commit with a clear
message, `git push -u origin <branch>`, and report the branch and the deploy URL when
you're done. Do not push to `main` unless Arfa says so in that session — `main` is the
production branch and pushing to it ships the site.

**Research before you write.** Use web search and page fetches to pull real listings,
duty schedules, LME/commodity prices and supplier pages. Every figure that lands on a
page must trace to something you actually opened, with the date you pulled it.

**No build tooling.** Don't add npm, a bundler, a framework, Tailwind, or a package.json.
Every page is one self-contained `.html` file with inline `<style>` and `<script>`.
Keep it that way.

**Never fabricate a fact.** Every price, MOQ, phone number, duty rate and URL on these
pages came from a real listing or source and is dated. If a number is an estimate or a
model, the page says so explicitly in a note. Do not add figures you haven't verified,
and do not quietly upgrade an estimate into a stated fact. If you can't verify something,
leave a `TODO` and tell Arfa.

**Don't break dead-link hygiene.** Alibaba retired its `/showroom/` and `/supplier/`
keyword paths — they return an empty "No results found" page. Use the live search
endpoint instead: `https://www.alibaba.com/trade/search?tab=all&searchText=...`
(or `tab=supplier`). Verify any new external link before adding it.

**Keep the repo root clean.** Vercel serves every file at the root as a public URL.
No zips, no `index_2.html`-style scratch copies, no exports. If a file isn't one of the
pages below, it doesn't belong at the root.

## Structure

- `index.html` — the engine dashboard: batch scoreboard, product cards, method, decisions
- `oodie.html` — Teardown #1: oversized wearable blanket (China vs India, landed cost)
- `ashtray.html` — Teardown #2: disposable gel ashtray (domestic-only, HORECA channel)
- `copper.html` — Teardown #3: copper import into India (forms, countries, duty, compliance)

All four must sit at the **repo root** — Vercel serves `index.html` as the homepage.
Nav links between pages are relative (`href="ashtray.html"`).

When you add a teardown, `index.html` is part of the change, not a follow-up: add the
nav link, the product card, the scoreboard row, and update the counts in the headline
and the tick strip. A teardown page that nothing links to is a page nobody reads.

## Design system — match it exactly on any new page

Copy the `<style>` block from an existing teardown as the starting point.

- Fonts: Fraunces (headings), Inter (body), IBM Plex Mono (data/labels), via Google Fonts
- Palette: `--paper:#FBF7F1` `--ink:#2B2320` `--mute:#7C6F66` `--line:#E8DFD3`
  `--terra:#B85042` (accent) `--sand:#EFE9DC` `--china:#3E5C76` `--india:#5F8468`
- Recurring components: `.cmp` (data table), `.slip` (card), `.callout`, `.ledger`
  (cost build-up), `.calc` (live calculator), `.chip` (verdict badge), `.rv` (scroll reveal)
- Every teardown ends with a `<footer>` containing a `.decide` block (decisions needed)
  and a `.credit` line naming every source and its date.
- Numbers are set in IBM Plex Mono. Indian formatting: ₹ with lakh/crore grouping.

## Conventions that carry the project's voice

- Lead with the finding, not the framing. Headlines state conclusions.
- Every teardown carries a verdict chip: BUILD / BUILD NEXT / TEST / SKIP, and the
  reasoning behind a SKIP is shown rather than hidden.
- Scores are Demand · Margin · Ops, each out of 5.
- Flag weaknesses in our own analysis on the page itself — modelled estimates, slab
  prices that aren't negotiated quotes, supplier diligence concerns.
- Contact sheets list real published numbers with a "verify on first call" note and
  the date they were pulled.

## Git

- Repo: `arshaowte-py/sourcing-engine`. Production branch: `main`.
- Commit only the `.html` files and this file. No zips, no build output, no `.vercel/`.
- `git push -u origin <branch>`; retry a network failure up to 4 times with backoff.
- Never force-push `main` and never rewrite pushed history.
- Open a PR only when Arfa asks for one.

## Deploy

Vercel, team `arshaowte-pys-projects` (`team_8isEpabCBHOpsd2S21LWIr2s`).
Framework preset "Other", root directory blank, no build command — the HTML ships as-is.

- **Production** is whatever is on `main`. Merging or pushing to `main` builds and goes live.
- **Preview**: deploy a branch from the session before asking Arfa to look at anything.
- Deploys are driven from the session via the Vercel MCP tools (`create_git_project`,
  `deploy_to_vercel`, `list_deployments`, `get_deployment_build_logs`), not the CLI.
- After a deploy, check the deployment reached `READY` and open the URL before reporting
  it. A build that Vercel accepted is not the same as a page that renders.

TODO (unverified as of 2026-08-20): no Vercel project for this repo appears under the
`arshaowte-pys-projects` team — the only project there is `fes-walk-ins-crm`. If the live
site is served from a different Vercel account, record the team and project here. If it
isn't linked yet, link the repo once and note the project ID.
