# Distribution Center — TTBC Short-Drama Distributor Workbench (Prototype)

An interactive, single-file HTML prototype of a **short-drama distributor workbench** hosted inside TikTok Business Center (TTBC). Distributors browse dramas that rights holders have opened for distribution, claim collections, generate distribution links for use in TTAM, track link health (with auto-pause when a link becomes invalid), and review, confirm, and track payouts for settlement statements.

**Live demo:** https://lexy-xy.github.io/distribution-platform/

## What's inside

Three business modules, all under **Creatives → Distribution Center** in the left nav:

- **Drama Marketplace** — browse distributable dramas and claim collections.
- **Distribution Management** — link view and by-drama view; copy/export links, monitor link validity and auto-pause results.
- **Settlement** — settlement statements, confirmation, and payout logs.

A **floating demo controller** (bottom-left, dashed yellow outline — not part of the product UI) switches between **4 views × 9 states**:

- Views: Drama Marketplace · Link view · By drama · Settlement
- States: Normal · Empty · Loading · Error · Partial success · Result unknown · Link invalid & auto-pausing · Settlement pending confirmation · Payout exception

Each view/state combination is deep-linkable, e.g. `#/distribution/links?state=invalid` or `#/settlement?state=payout`, and the controller's **Copy demo link** button copies the current one.

## Screenshots

![Drama Marketplace](screenshots/marketplace.png)

![Distribution Management — link invalid, auto-pausing](screenshots/link-invalid-autopause.png)

![Settlement](screenshots/settlement.png)

More in [`screenshots/`](screenshots/).

## Run locally

No build step or backend; plain HTML/CSS/JavaScript.

```bash
git clone https://github.com/lexy-xy/distribution-platform.git
cd distribution-platform
open index.html            # macOS (or just double-click the file)
# or serve it:
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Repository layout

| Path | Contents |
| --- | --- |
| `index.html` | The full prototype (single file) |
| `docs/requirements-context.md` | Requirements analysis and generation context (Chinese) |
| `screenshots/` | Reference screenshots of key views and states |
