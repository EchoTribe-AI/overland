# Session Log — Display Ads / NextMillennium Workstream

Working log for the display monetization effort on expeditionportal.com and
forum.expeditionportal.com. Branch: `claude/display-ads-video-fill-c3kevv`.

---

## Session: Jul 23–30, 2026 — Video fill investigation → NM call prep

### Context

Started from the question of why NextMillennium video fill was low and how to
optimize display ahead of the NM meeting (week of Aug 3). Expanded into a full
mapping of the ad stack after audit data showed the problem was structural.

### Deliverables produced (all on this branch)

| File | Purpose | Status |
|---|---|---|
| `display-ops-field-manual.html` | Plain-English reference for the team: the GAM waterfall, assets, levers, and metrics glossary (Session RPM as north star). Written for teammates new to display. | Final |
| `display-placement-integration-blueprint.html` | Placement-by-placement map of what GAM vs. NM fills, and the plan to wire direct-sold → NM backstop → AvantLink into one auction. **Rev 2 (Jul 30)** corrected four claims against first-party verification and added the Montana before/after exhibit. | Rev 2 |
| `nm-call-agenda.html` | Internal call prep: non-negotiables, verified facts, eight ordered questions, outcomes checklist, confirmed-vs-assumed tracking. | Final |
| `nm-call-agenda-shared.html` | Neutral shareable version of the agenda (for Jake / the meeting itself) — facts, discussion topics, desired outcomes only. | Final |

### Key findings (as verified through Jul 30)

1. **Two silos, no bridge.** GAM (network 10039233) and NM (publisher 15113,
   powerad.ai wrapper) run completely separate auctions. GAM has zero yield
   groups/bidders/NM line items; NM only monetizes the 4 units its own script
   injects (placement 4740 ×2 in-article, 60165 + 8045 overlays). GAM's unsold
   inventory has **no paying backstop** — its only "programmatic" is 8 dead
   AdX line items ($0 since 2019).
2. **The backstop gap was observed live.** Jul 23: article slots filled by the
   Montana State direct flight (548% pacing). Jul 30: flight completed, same
   real estate now house/AvantLink at near-$0. This is Exhibit A for the call.
3. **"Video revenue" was never a video unit.** No video ad unit exists in GAM;
   NM's "video" placement generates 80% of all requests (22.8M of 28.5M YTD)
   at ~7.9% fill and $0.75 eCPM — request bloat, not demand. Request volume is
   outbound, not buyer interest.
4. **A Connatix outstream player already runs in the article sidebar**
   (discovered Jul 30). connatix.com sits DIRECT in NM's managed ads.txt
   block, but its revenue is not broken out anywhere visible. Whether NM's
   "video" placement *is* the Connatix auction is an open question for the
   call. Video strategy reframed: audit the existing player before building
   a new one.
5. **The NM wrapper is scroll-gated (>3 scroll events)** — solved the earlier
   "wrapper loads but never auctions" mystery. First impression always goes to
   GAM. Desktop Chrome is healthy (63% of NM revenue); Safari ($0.76) and
   in-app ($0.79) eCPMs drag the ~$28/day, $0.90–1.13 blend.
6. **ads.txt was stale and we fixed it ourselves (Jul 26).** Live file was
   NM's 12-2024 test file missing 56 seller records; corrected 270-record file
   deployed; reporting normalized within a day. Forum subdomain has no ads.txt
   (404) — needed before forum enablement, likely benign today.
7. **Forum: zero NM placements** (config returns none for any forum URL);
   legacy ~2018 GAM slots only. Slot-count discrepancy open: 8 GPT slots
   (Jul 23 count) vs ~29 non-NM slots (Jul 30 count) on article pages —
   arbitrate via GAM ad-unit report before quoting inventory to NM.
8. **AvantLink 20%→5% is moot until the bridge exists.** AvantLink (Network,
   priority 12, 20% of remaining, ~$0 effective value — 82 clicks/292K imps)
   and NM never meet inside GAM, so cutting its share today just creates
   blanks. Sequence: build NM→GAM bridge first, then reposition AvantLink to
   House/promotional layer, then test on Session RPM.

### Decisions / direction agreed

- Target stack: **direct-sold (untouchable priority) → NM as in-auction
  backstop (Prebid line items preferred, Open Bidding acceptable) → AvantLink
  as deliberate promotional layer → video as its own audited lane.**
- Non-negotiables for the NM relationship: direct-sold priority stays, we keep
  GAM admin control and independent reporting, every change gets a projected
  lift and a 2-week incremental Session RPM validation. Managed/MCM offers:
  unsold-only scope at most.
- Reporting access (placement + device level) turns on **before** other
  changes so a baseline exists.

### Open items

- [ ] NM call: run the agenda; the must-have outcome is an agreed integration
      mechanism (Prebid/OB) with a setup date.
- [ ] Resolve Connatix attribution and the "video"-placement mapping.
- [ ] GAM ad-unit report to settle the 8 vs ~29 article slot count.
- [ ] Montana State: click-tracking post-mortem (0 clicks / 933K imps) and
      sponsor make-good before renewal conversation.
- [ ] Hankook 2026: still "needs creatives" — upload or pause.
- [ ] Deploy forum ads.txt ahead of forum enablement.
- [ ] After bridge: reposition AvantLink, run the 2-week Session RPM test.
- [ ] Roadmap: owned-content instream (XOVERLAND cut-downs + pre-roll), gated
      on an editing pipeline and a rights check.

### Published artifacts (private; share via each page's Share menu)

- Field Manual: https://claude.ai/code/artifact/46e5d5b6-7af8-4492-8872-487550548432
- Blueprint (Rev 2): https://claude.ai/code/artifact/3f053f25-6364-4441-8292-ab13b2793c2b
- Call Agenda (internal): https://claude.ai/code/artifact/55fea31b-7c03-432b-a79f-168fa526d4b9
- Meeting Agenda (shared): https://claude.ai/code/artifact/8ccb4d72-2762-44bf-886b-675a441d5e99

---

## Session: Jul 31, 2026 — NM call debrief + historical revenue analysis

### New inputs
- NM call transcript (Jul 30) and Kelly's follow-up email to NM.
- NM placement exports for **2023** and **2024** (revenue, requests, impressions,
  eCPM by placement).
- Brian confirmed the forum ad pause was caused by **community complaints about
  banners injected between posts** — not a blanket objection to forum ads.

### Deliverable
`nm-revenue-opportunity.html` — historical decline analysis and sized recovery
plan, rated for UX cost.

### Headline finding

**The revenue decline was inventory removal, not yield failure.**

| Period | Revenue | Impressions | Fill | eCPM |
|---|---|---|---|---|
| 2023 actual | $37,356 | 18.7M | 48.5% | $2.00 |
| 2024 actual | $25,837 | 12.0M | 40.0% | $2.15 |
| 2025 | **not provided — request from NM** | | | |
| 2026 annualized | $10,199 | 8.2M | 15.9% | $1.25 |

- **2023→2024:** every eCPM was flat or up (blended $2.00 → $2.15), yet revenue
  fell $11,519. gunstar and eagle alone gave up **$13,174** on lost volume, with
  fill rates unchanged (gunstar 38.2%→37.9%, eagle 56.7%→55.9%). That is the
  signature of pages removed from placement scope — i.e. the forum exclusion.
  Traffic decline doesn't fit: prometheus requests *rose 143%* the same year.
- **2024→2026:** different pattern — non-video impressions −57% *and* eCPM −25%
  ($2.11 → $1.58). Yield half likely the ads.txt breakage (file stamped
  **12-18-2024**, exactly the boundary); volume half likely the loader scroll
  gate (non-video requests 28.5M → ~10.2M annualized while fill held ~48%).
- **video placement:** revenue flat near $2K/yr across all three periods while
  requests grew 39× (1.06M → 41.0M annualized). Pure overhead; retiring it costs
  at most ~$2,400/yr.

### Opportunity (annual, above the ~$10.2K baseline)

| Initiative | Estimate | UX cost | Confidence |
|---|---|---|---|
| Forum re-enabled under guardrails | $5.0–8.7K | Low | Medium-high |
| Video pre-roll (owned content, 5s skippable) | $3.0–16.0K | Low | Low — needs video traffic data |
| GAM backfill tag | $3.0–9.0K | None | Low on size, high on direction |
| Playwire migration | $3.0–7.0K | None | Low |
| ads.txt fix (done Jul 26) | $1.5–2.6K | None | Medium |
| Loader timing repair | unsized | None if viewport-based | Possibly the largest item |

Conservative total ≈ **$23K**, optimistic ≈ **$50K**. 2023 was $37,356 — so the
historical peak is reachable **without any high-impact format**.

### Position on formats
- **Declining:** in-flow forum units, sticky mobile anchors, floating/expanding
  video, interstitials.
- **Accepting:** sidebar + below-content forum units (lazy-loaded, per-page cap),
  5s skippable pre-roll on owned video, backfill in existing GAM slots.
- **Push back on** NM's "load ads as fast as possible" — the answer is
  viewport-triggered lazy loading, not eager loading and not the current
  scroll-count gate.

### Data requests that would resize the model
- [ ] GA4 sessions for 2023 / 2024 / 2026 — separates loader gating from real
      traffic decline (the single most important number).
- [ ] 2025 full-year NM report (missing year).
- [ ] GAM unfilled impressions by ad unit (30/90 day).
- [ ] On-site XOVERLAND video page traffic.
- [ ] Exact date the forum exclusion was applied.

### Next actions
- [ ] Compare August eCPM vs June to measure the ads.txt fix.
- [ ] Install GAM backfill tag behind direct-sold, ahead of AvantLink — this also
      makes the long-pending AvantLink 20%→5% question meaningful at last.
- [ ] Stage and review the Playwire preview build against the format guardrails.
- [ ] Deploy forum ads.txt, then re-enable forum with sidebar/below-content only.
- [ ] Replace scroll-count gate with viewport-triggered loading.
- [ ] Video pre-roll once Aaron's integration details arrive; retire in-banner
      video calls at the same time.

### Published artifact
- Revenue Opportunity: https://claude.ai/code/artifact/eb9bbc67-b3b4-4bac-9729-1b3460e3c9a9
