# Coherence decks sandbox

A safe place to restyle and reformat the Coherence **Investment** and **Partnerships** decks so they look like the **Info** deck, before anything goes live.

- Sandbox link (for review): https://ugla-ctrl.github.io/coherence-decks-sandbox/
- Live decks (do not edit from here):
  - Info (style reference): https://info.coherenceatx.com
  - Investment: https://investment.coherenceatx.com (repo `ugla-ctrl/coherence-deck`, branch `master`)
  - Partnerships: https://partnerships.coherenceatx.com (repo `ugla-ctrl/coherence-partnership-deck`, branch `main`)

## What is here

```
index.html            landing page with the links (noindex)
investment/           working copy of the Investment deck (index.html + media/)
partnerships/         working copy of the Partnerships deck (index.html + media/)
```

## How it is set up (important)

1. **Baseline commit**: exact byte-for-byte copies of the live decks, taken from
   `coherence-deck@2832633` (Investment) and `coherence-partnership-deck@d33d0b5` (Partnerships) on Oct 8, 2026.
2. **Sandbox adaptation commit**: removes the production email gate and the view tracking (they write to the live Supabase
   tables, so reviews would otherwise show up as leads and views in the Daily Site Access Report), hides the gate overlay,
   and adds `noindex`. Nothing else changes.
3. **Everything after that** is restyling work. That is the only part that should go to production.

There is deliberately **no `CNAME` file** here. Adding one would claim a live domain.

## Try it locally

```bash
python3 -m http.server 8000      # then open http://localhost:8000/
```

## Deploying later (port the restyle, keep the live gate)

Do not copy these folders over the production repos: that would delete the live email gate and tracking.
Port only the restyle commits instead. Replace `ADAPT` with the sandbox adaptation commit id (the second commit).

```bash
# in this repo
git diff ADAPT..HEAD -- investment/ > /tmp/investment.patch
git diff ADAPT..HEAD -- partnerships/ > /tmp/partnerships.patch

# in the production repos (they keep index.html at the top level, so strip two path levels)
cd ../coherence-deck && git apply -p2 --check /tmp/investment.patch && git apply -p2 /tmp/investment.patch
cd ../coherence-partnership-deck && git apply -p2 --check /tmp/partnerships.patch && git apply -p2 /tmp/partnerships.patch
```

New files (images, fonts) in the patch are added too. If the production deck changed in the meantime (for example Patrick's
content updates), `git apply` will say so and the conflicts are fixed by hand.

## Rules of the sandbox

- Edit only here. The live decks stay untouched until a deploy is approved.
- Keep it free of the gate and tracking snippets.
- This site is unlisted and not indexed, but it is a public web address: anyone with the link can open it.
