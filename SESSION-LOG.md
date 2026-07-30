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
