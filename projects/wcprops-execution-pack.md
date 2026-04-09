# WCProps — Execution Pack
_Last updated: 2026-03-29 | Status: Awaiting inputs before deploy_

---

## SECTION 1 — Required Inputs Before Anything Deploys

These four items are blockers. Nothing ships without them.

| # | Input | Why It's Needed | Status |
|---|---|---|---|
| 1 | **Picks destination URL** | All 128 CTAs across the site point to Polymarket. Need the real target URL. | ❌ Missing |
| 2 | **GA4 Measurement ID** (format: `G-XXXXXXXXXX`) | No analytics on any page. Every day without it is permanent data loss. | ❌ Missing |
| 3 | **GitHub repo access** | All file edits require repo access. Token `gia-ctrl` authenticates but sees 0 repos. Need: org name, repo name, or invite. | ❌ Missing |
| 4 | **Vercel team access** | To deploy after edits, to review env vars, to confirm domain config. Need: Vercel team invite or login. | ❌ Missing |

**Minimum viable unlock:** GitHub repo access + GA4 ID. Those two unblock 90% of Day 1.

---

## SECTION 2 — Changes by Priority

### First 30 Minutes
_Requires: GitHub repo access + GA4 ID_

**Action 1 — Install GA4 (all pages)**

Find the exact insertion point: line 441–442 in every HTML file, immediately before `</style>\n</head>`.

Insert this block:
```html
  <!-- Google Analytics -->
  <script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', 'G-XXXXXXXXXX');
  </script>
```

Replace `G-XXXXXXXXXX` with the real ID. This is a single global find/replace on the string `</style>\n</head>`.

**Action 2 — Fix Mbappe Value Pick**

File: `world-cup-2026/players/kylian-mbappe/index.html`

Find:
```
Pulisic's 0.74 G/90 rate this season, combined with being the primary creator and penalty taker for a home nation expected to go deep, makes this line hittable. In 2022 he scored the biggest goal of the tournament for the US despite only playing ~240 minutes. With a projected 420+ minutes in 2026, the math favors the over.
```

Replace with:
```
Mbappe's 0.74 G/90 this season at Real Madrid is the best rate of his career. He has 12 goals in 14 World Cup appearances — an extraordinary strike rate across two tournaments. As France's penalty taker, primary creator, and the consensus Golden Boot favorite, he's projected 500+ minutes if France reach the semi-finals as expected. At 3.5 goals, history and current form both say over.
```

**Action 3 — Run copy audit across all 112 player pages**

Run in repo root:
```bash
for file in world-cup-2026/players/*/index.html; do
  player=$(echo "$file" | sed 's|world-cup-2026/players/||;s|/index.html||')
  vp=$(grep -A2 "Value Pick" "$file" | grep "<p>")
  echo "$player | $vp"
done | grep -v "^$"
```

Flag every entry where the player name in the copy doesn't match the file name. These are the broken pages. Fix before deploying.

**Deploy. Verify GA firing.**

---

### First 2 Hours
_Requires: GitHub repo access + Picks URL_

**Action 4 — Replace all Polymarket CTAs with Picks CTAs (bulk)**

128 total instances across player pages and team pages.

Global find/replace #1 — the CTA button text and link:
```
FIND:    href="https://polymarket.com/event/2026-fifa-world-cup-winner-595" class="cta-btn" target="_blank" rel="noopener">Trade on Polymarket</a>
REPLACE: href="[PICKS_URL]" class="cta-btn">Get Early Access →</a>
```

Global find/replace #2 — the CTA subtitle copy:
```
FIND:    <p>Transparent markets. No house edge. Verifiable payouts.</p>
REPLACE: <p>Player props. P2P markets. No house edge. Your prediction, your payout.</p>
```

The `<h3>Pick [player] props</h3>` line is already player-specific and correct — leave it. Just update the text to read "Pick [player] on Picks" where it currently says "Pick [player] props".

Global find/replace #3 — h3 title pattern:
```
FIND:    <h3>Pick (\w+) props</h3>
REPLACE: <h3>Pick \1 on Picks</h3>
```
Use regex replace. Works across all files in one pass.

**Action 5 — Add above-fold announcement bar (all pages)**

Find the exact anchor:
```
FIND:    <div class="flag-banner"></div>
REPLACE: <div class="flag-banner"></div>

  <div style="background:#1a1a1a;padding:0.6rem 2rem;text-align:center;font-size:0.8rem;display:flex;align-items:center;justify-content:center;gap:1rem;flex-wrap:wrap;">
    <span style="color:#aaa;">World Cup 2026 player props are coming to Picks.</span>
    <a href="#notify-form" style="background:#00b894;color:white;padding:0.3rem 1rem;border-radius:20px;font-weight:700;font-size:0.75rem;text-decoration:none;white-space:nowrap;">Get Early Access →</a>
  </div>
```

If Picks URL is live, replace `#notify-form` with the real URL.

**Action 6 — Update email capture copy (all pages)**

Global find/replace #1:
```
FIND:    Get notified when player props go live
REPLACE: Get early access to Picks
```

Global find/replace #2:
```
FIND:    We'll email you when prop markets open for World Cup 2026
REPLACE: Player props. No house edge. P2P markets. Launching for World Cup 2026 — join the list.
```

Global find/replace #3:
```
FIND:    >Notify Me</button>
REPLACE: >Join the List →</button>
```

**Deploy. Spot-check 5 player pages.**

---

### Day 1 (remaining)

**Action 7 — Fix breadcrumb team links (per-player, batch by team)**

Currently: breadcrumb shows `Players / Kylian Mbappe` (no team link)
Target: `France / Kylian Mbappe` (linked to team page)

This requires per-file edits, batched by team. Process:

```bash
# France players
for player in kylian-mbappe antoine-griezmann ousmane-dembele aurelien-tchouameni mike-maignan william-saliba; do
  sed -i 's|<a href="/world-cup-2026/players/">Players</a>|<a href="/world-cup-2026/teams/france/">France</a>|g' \
    world-cup-2026/players/$player/index.html
done
```

Repeat pattern for each of the 16 teams. Full player-to-team mapping:

| Team | Players in sitemap |
|---|---|
| Argentina | lionel-messi, julian-alvarez, enzo-fernandez, emiliano-martinez, cristian-romero, alexis-mac-allister, facundo-pellistri, federico-valverde, rodrigo-bentancur, ronald-araujo, darwin-nunez |
| England | harry-kane, jude-bellingham, bukayo-saka, phil-foden, declan-rice, trent-alexander-arnold |
| France | kylian-mbappe, antoine-griezmann, ousmane-dembele, aurelien-tchouameni, mike-maignan, william-saliba |
| Spain | pedri, dani-olmo, lamine-yamal, nico-williams, rodri, unai-simon |
| Germany | jamal-musiala, florian-wirtz, joshua-kimmich, antonio-rudiger, manuel-neuer, kai-havertz |
| Brazil | vinicius-junior, rodrygo, raphinha, marquinhos, bruno-guimaraes, alisson |
| Portugal | cristiano-ronaldo, bernardo-silva, bruno-fernandes, ruben-dias, rafael-leao, diogo-costa |
| Netherlands | virgil-van-dijk, frenkie-de-jong, xavi-simons, cody-gakpo, nathan-ake, bart-verbruggen |
| Belgium | romelu-lukaku, kevin-de-bruyne, jeremy-doku, amadou-onana, arthur-theate, koen-casteels |
| Croatia | luka-modric, mateo-kovacic, marcelo-brozovic, josko-gvardiol, dominik-livakovic, andrej-kramaric |
| Morocco | achraf-hakimi, hakim-ziyech, sofyan-amrabat, yassine-bounou, nayef-aguerd, youssef-en-nesyri |
| USA | christian-pulisic, weston-mckennie, tyler-adams, gio-reyna, tim-weah, yunus-musah |
| Colombia | james-rodriguez, luis-diaz, jhon-duran, davinson-sanchez, richard-rios, camilo-vargas |
| Uruguay | federico-valverde*, darwin-nunez*, rodrigo-bentancur*, sergio-rochet, ronald-araujo* | 
| Switzerland | granit-xhaka, xherdan-shaqiri*, yann-sommer, manuel-akanji, breel-embolo, denis-zakaria, dan-ndoye |
| Japan | takefusa-kubo, kaoru-mitoma, ritsu-doan, wataru-endo, ko-itakura, shuichi-gonda |

*Verify team assignment before editing — a few players appear in multiple squad lists

**Action 8 — Review and fix remaining copy errors from audit (Action 3)**

Page-by-page fixes for any mismatched value pick copy found in the audit. Each requires reading the page and writing accurate player-specific copy.

---

## SECTION 3 — Bulk Operations vs Page-by-Page

### Can be done via global find/replace (all 128 files at once)
- GA4 tag installation
- Polymarket CTA URL replacement
- CTA button text (`Trade on Polymarket` → `Get Early Access →`)
- CTA subtitle copy
- H3 CTA title pattern (`Pick X props` → `Pick X on Picks`)
- Email capture headline copy
- Email capture subtext copy
- Button text (`Notify Me` → `Join the List →`)
- Announcement bar insertion (after `flag-banner` div)

### Requires page-by-page review
- Value pick copy errors — each broken page needs correct player-specific text written
- Breadcrumb team assignment — correct team slug per player
- Any stat errors or factual inaccuracies found during audit
- Building 32 missing team pages (new content, not edits)

---

## SECTION 4 — Risk Controls

### Before any edit

**Mandatory: full repo snapshot**

```bash
# In the repo root — before touching anything
git status                          # confirm clean working tree
git log --oneline -10               # note the current HEAD commit hash
git stash                           # if any uncommitted changes exist

# Create a backup tag
git tag backup-pre-execution-$(date +%Y%m%d) HEAD
git push origin --tags              # push tag to remote (read-only for now — do after write access confirmed)
```

If no git access: download a full ZIP of the repo from GitHub before any edits.

**Minimum backup required:** The current HEAD commit hash. If anything breaks, `git revert` to this commit and everything is restored in one command.

### After deployment — verification checklist

Run these checks in order. If any fails, halt and investigate before continuing.

**Check 1 — Site is still up:**
```bash
curl -s -o /dev/null -w "%{http_code}" https://wcprops.app/world-cup-2026/
# Expected: 200
```

**Check 2 — No 500 errors on key pages:**
```bash
for url in \
  "https://wcprops.app/world-cup-2026/" \
  "https://wcprops.app/world-cup-2026/players/kylian-mbappe/" \
  "https://wcprops.app/world-cup-2026/players/lionel-messi/" \
  "https://wcprops.app/world-cup-2026/teams/spain/"; do
  status=$(curl -s -o /dev/null -w "%{http_code}" "$url")
  echo "$status $url"
done
# All should return 200
```

**Check 3 — Mbappe fix is live:**
```bash
curl -s "https://wcprops.app/world-cup-2026/players/kylian-mbappe/" | grep -c "Pulisic"
# Expected: 0 (no matches)
curl -s "https://wcprops.app/world-cup-2026/players/kylian-mbappe/" | grep -c "Mbappe's 0.74"
# Expected: 1
```

**Check 4 — GA is firing:**
- Open GA4 → Reports → Realtime
- Open https://wcprops.app/world-cup-2026/players/kylian-mbappe/ in a browser
- Within 30 seconds, active user count should increment
- Expected: 1+ active user visible

**Check 5 — No Polymarket CTAs remain:**
```bash
curl -s "https://wcprops.app/world-cup-2026/players/kylian-mbappe/" | grep -c "polymarket.com"
# Expected: 0
curl -s "https://wcprops.app/world-cup-2026/world-cup-2026/" | grep -c "polymarket.com"
# Check homepage — Polymarket market widget on homepage is separate, leave it
```

**Check 6 — Picks CTA is present:**
```bash
curl -s "https://wcprops.app/world-cup-2026/players/kylian-mbappe/" | grep -c "Get Early Access"
# Expected: 1+
```

**Check 7 — Internal links work:**
```bash
# Breadcrumb team link on Mbappe page should return 200
curl -s -o /dev/null -w "%{http_code}" "https://wcprops.app/world-cup-2026/teams/france/"
# Expected: 200
```

**Check 8 — Sitemap still valid:**
```bash
curl -s https://wcprops.app/sitemap.xml | grep -c "<url>"
# Expected: same count as before (114) or higher if new pages added
```

---

## SECTION 5 — Success Criteria

### Mbappe page is fixed ✓
- `curl ... | grep "Pulisic"` returns 0
- Value pick paragraph references Mbappe's stats and France's tournament path
- Page loads without errors (HTTP 200)

### GA is live ✓
- GA4 Realtime shows active users within 60 seconds of page load
- GA4 DebugView shows `page_view` event firing on at least 3 different pages
- No console errors related to gtag in browser devtools

### Picks CTA is live ✓
- No `polymarket.com` in any player page CTA button
- CTA text reads "Get Early Access →" on all player pages
- CTA links to Picks URL (or `#notify-form` if URL not yet available)
- Button is visible above the fold on mobile (375px width test)

### Internal linking works ✓
- Breadcrumb on Mbappe page: `World Cup 2026 / France / Kylian Mbappe` — France is a working link
- Clicking France link in breadcrumb loads `/world-cup-2026/teams/france/` (HTTP 200)
- Team page (Spain example): all 6 key player names are clickable links that resolve to 200
- No broken links (HTTP 404) in breadcrumbs or related sections

---

## Open Items (awaiting your input)

| Item | Question |
|---|---|
| Picks URL | What domain/URL should all CTAs point to? |
| GA4 property | Do you have one already? If yes, what's the Measurement ID? If no, create at analytics.google.com |
| GitHub | Which account/org holds the WCProps repo? Or share the repo URL directly. |
| Vercel | Do you have login access to the Vercel project `prj_q9dOEmVc2eNmg4WnmiYhzddZMKGp`? |
| Picks homepage | Is there a live Picks landing page or product URL yet? |

---
_This document is the single source of truth for WCProps execution. Update as inputs arrive._
