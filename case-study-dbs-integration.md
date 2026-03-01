# How I Orchestrated a Major Institutional Lending Integration from Zero to Production

**Role:** Product Manager · **Timeline:** January – August 2024 · **Impact:** Doubled total disbursement volume · Multi-hundred billion IDR credit facility unlocked

---

## The Stakes

When I was assigned to this project, our funding situation was critical. With one active channeling partner in place, expanding our institutional funding base was a strategic priority — diversifying our lending capacity and creating room for controlled, sustainable growth. The pressure to deliver was immediate.

To make things harder: the senior PM who had been leading this project left the company mid-way. There was no formal handover. I inherited fragmented documentation, fragmented institutional knowledge, and a project already in motion — and was expected to drive it to completion.

It was my first time leading an integration of this scale. I had to learn everything on the fly.

---

## What I Was Responsible For

This wasn't a single-track PM role. I was the orchestrator across the entire integration lifecycle:

- **Data mapping and structure** — aligning our internal data models with the lender's requirements
- **UAT coordination** — managing test scenarios across both sides of the integration
- **Money flow design** — mapping how funds would move from lender to borrower and back
- **Production release** — coordinating the go-live across engineering, operations, and the lender's team
- **Post-launch maintenance** — owning operational stability including callback monitoring and reconciliation

---

## The Pivot That Reset Everything

Midway through the project, after the SFTP-based flow had already been scoped and largely designed, the lender requested a full shift to API-based integration. We had no bargaining power — institutional lenders set the terms. So we pivoted.

It pushed the timeline back by one to two months and meant reworking the money flow from scratch. The original SFTP work wasn't wasted, but the architectural change required re-aligning the entire team's understanding of how the integration would function.

This is where having no prior experience in this type of integration actually sharpened my instincts. With no assumptions to fall back on, I had to reason through every decision from first principles — and document everything as I went, building the knowledge base that hadn't existed when I inherited the project.

---

## Navigating Post-Launch

Going live in August wasn't the finish line. Institutional lending integrations require continuous operational attention — callback success rates, disbursement reconciliation, and exception handling all needed active monitoring and rapid response.

I maintained ownership of this through post-launch, identifying and resolving operational issues as they surfaced, ensuring disbursements continued running without disruption.

---

## The Result

Eight months from a standing start — through a mid-project pivot, a leadership gap, and a steep learning curve — the integration went live and has remained stable since.

> **Doubled total disbursement volume** across our lending operations
> **Multi-hundred billion IDR credit facility** unlocked and operational
> **99.9% uptime** maintained since production launch
> **8 months** from project inheritance to go-live, including a full architectural pivot

---

## What I Took Away

The hardest part of this project wasn't the technical complexity — it was operating with confidence in a high-stakes environment where I had no safety net, no prior experience, and no complete map of the territory. I had to build the map while moving.

What carried me through was treating every unknown as a structured problem: identify what I don't know, find who does, document what I learn, and keep the integration moving forward. That approach — more than any specific skill — is what I'd take into any complex cross-functional project.
