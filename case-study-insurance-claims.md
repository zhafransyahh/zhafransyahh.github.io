# Building an Insurance Claims System from Scratch for Institutional Lending

**Role:** Product Manager · **Timeline:** 2024 · **Impact:** Hundreds of billions IDR in loan portfolio covered · End-to-end 0-to-1 system design and ownership

---

## The Starting Point: Nothing

When our institutional lending partner required insurance coverage for all loans disbursed through our platform, there was no system to point to. No process, no integration, no documentation. Just a regulatory requirement and a blank page.

I designed and shipped it from scratch — and continue to own it independently today.

---

## How the System Works

The insurance flow I designed covers the entire lifecycle of a loan, from disbursement to claim resolution:

1. **Enrollment** — When a loan is disbursed, it is automatically enrolled into insurance coverage
2. **Premium calculation** — Enrolled loans determine the premium JULO pays to our insurance partner, calculated with a financial adjustment mechanism to ensure accuracy at scale
3. **Claim trigger** — When a loan reaches a defined delinquency threshold, a claim is filed
4. **Claim confirmation** — Our insurance partner processes the claim and sends a confirmed amount directly to the lender

The system runs as an API integration across three parties: our internal platform, the insurance partner, and the institutional lender — coordinated across engineering, QA, business, and two external organizations simultaneously.

---

## The Hardest Part: Getting the Numbers Right

The most technically demanding aspect of this system was claim accuracy. Insurance claims are based on outstanding loan amounts and interest at the time of the claim — but these numbers change daily. A claim filed against a moving target risks rejection.

My solution was to implement a cutoff-date snapshot mechanism. At the point of claim, the system captures a fixed snapshot of the outstanding balance and applicable interest, locking those figures before the claim is processed. This eliminated the risk of calculation drift causing claim rejections downstream.

Getting this right required close coordination with engineering to ensure the snapshot logic was airtight, and with the business team to validate that the financial adjustment calculations were accurate at scale — where even small errors compound significantly across a large portfolio.

---

## Ownership

This system runs entirely under my ownership — I monitor it, maintain it, and continue iterating on it independently.

Current claim processing takes approximately one week end-to-end. I'm actively working on automation improvements to bring that down to one day — reducing manual touchpoints and accelerating the confirmation loop between all three parties.

---

## The Result

> **Hundreds of billions IDR in loan portfolio** enrolled and covered since launch
> **0 to 1** — designed, integrated, and shipped with no prior system or precedent
> **Full-stack ownership** across design, integration, operations, and ongoing maintenance
> **Claim rejection risk eliminated** through snapshot-based calculation architecture
> **Active automation roadmap** to reduce claim processing from 7 days to 1

---

## What I Took Away

Building a 0-to-1 system that sits at the intersection of a fintech platform, a regulated insurance product, and an institutional lender's requirements forced me to think in systems — not features. Every design decision had downstream consequences for accuracy, compliance, and operational load.

The lesson I carry from this: in financial systems, correctness isn't a nice-to-have. It's the product. And the PM's job is to make sure every stakeholder — technical and non-technical — shares that understanding before a single line of code is written.
