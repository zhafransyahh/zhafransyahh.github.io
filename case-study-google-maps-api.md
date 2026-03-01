# How I Cut Google Maps API Costs by 99% Through a UX Fix

**Role:** Product Manager · **Timeline:** Q3–Q4 2024 · **Impact:** ~$13,500 saved in 3.5 months · $46K+ annualized run-rate

---

## The Anomaly

It started with a chart that didn't make sense.

While reviewing our mobile action logs, I noticed a sharp spike in Google Places API usage within our loan application flow. Before the spike, we were averaging a few hundred requests per day. Within weeks, that number had climbed by over 5,600% — seemingly overnight.

No change in user volume explained it. So I started digging.

---

## Finding the Root Cause

I pulled a time-series query from our internal logs to isolate exactly when the spike began and how it distributed across users.

The timing correlated precisely with a recent app release — one that standardized our main product's onboarding form address fields with structured dropdowns instead of free text. What made this tricky to catch early: the dropdown implementation wasn't new. It had already existed in our starter product for some time, where it worked without issue. But the starter product operates at a fraction of the volume of our main product, so the API cost it generated was small enough to go unnoticed.

When the same implementation was carried over to the main product, the scale difference made all the difference. The same flow that was invisible at low volume became a serious cost problem at scale — a 5,600%+ spike in daily API requests, almost overnight.

The culprit was in the flow itself. When users entered the address screen, the implementation forced them directly into a search interaction — triggering a Google Places API call immediately, before they'd done anything intentional. Users who fumbled through the screen or re-entered it multiple times were racking up 10, 15, even 20+ API hits per session. That cohort alone accounted for roughly 50% of total spike volume.

At standard Google Maps Platform pricing and a sustained peak rate, we were on track to spend over $4,500 USD per month if left unresolved.

---

## The Fix: Changing the Flow, Not the Feature

The instinct might have been to roll back the dropdown standardization entirely. I pushed back on that. The dropdown was the right product decision — it reduced input errors and improved data quality downstream. The problem wasn't the feature; it was where in the flow we were triggering the API.

Working with our designer and tech lead, we redesigned the address screen so that:

- Users land on a screen showing their address fields and a "Use Current Location" button — no search triggered yet
- The map and search bar only appear when a user actively taps into the location picker
- Address details fill in cascadingly once a pin is confirmed
- An error state handles location failures gracefully, with a manual fallback

The change moved search from a default behavior to an intentional one. No new infrastructure, no rollback.

---

## The Result

The fix went live in mid-Q3 2024. Daily API requests dropped immediately back to pre-spike baseline levels.

Over the following months, the optimization prevented approximately **4.79 million unnecessary API calls**, translating to:

> **~$13,500 USD in direct savings over 3.5 months**
> **$46,000+ USD annualized run-rate savings**
> API volume reduced by ~99% · Dropdown standardization fully retained

---

## What I Took Away

The technical fix took a few weeks. The harder work was in the diagnosis — recognizing that a cost problem was actually a UX flow problem in disguise, and that the right solution wasn't reverting a good feature but rethinking how users encountered it.

It reinforced something I think about often: infrastructure consequences are almost never visible at the time of a product decision. The PM's job is to make them visible before they become expensive — and in this case, the data made that possible.
