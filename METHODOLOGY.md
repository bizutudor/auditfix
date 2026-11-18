# Audit Methodology

The audit checks public, read-only signals. It scans a bounded sample and labels coverage so a partial scan is never presented as a full-site conclusion.

## Scores

Every automatic deduction corresponds to a finding shown in the report. Items that cannot be verified from outside (a robots.txt that times out, links that block bots, consent behaviour, tracking correctness, Google Business Profile, NAP consistency, legal adequacy) are listed under "Requires manual review" and do **not** lower any score.

The overall score is the arithmetic mean of the category scores that apply, rounded and clamped to 0-100:

1. Technical SEO: valid HTTPS certificate (-20) and HTTP-to-HTTPS redirect (-10), robots.txt and an XML sitemap that really are what they claim to be (an HTML "not found" page served at /sitemap.xml counts as missing), the www/non-www variant leading to the host the site actually uses, viewport, indexing directives, canonical tags, redirect chains with more than one hop, readable paths, invalid JSON-LD, linked or listed URLs that answer with an HTTP error (counted once, never as "missing title/H1"), and bounded link-health checks.
2. On-page SEO: title, meta description, H1, canonical, image alt coverage and text depth across the pages that answered successfully.
3. Conversion: forms, phone, email, WhatsApp, calls to action and contact-page availability.
4. Local SEO: contact details, direct contact paths, local schema and page language; only scored when local relevance is medium or high.
5. Ads readiness: HTTPS, privacy and terms pages, contact paths and measurement. No measurement tag at all costs more (-25) than tags without an observable default-denied consent state (-12). Because a public crawl cannot prove conversions or platform settings, this score is capped at 85.
6. Speed (lab, mobile): Core Web Vitals measured in the audit browser on the homepage and the next most important pages, simulating a mid-range phone on 4G (150 ms RTT, 1.6 Mbps down, 750 Kbps up) with a 4x CPU slowdown. LCP, CLS, TBT and FCP are scored with Lighthouse's published log-normal curves and weights (Speed Index is not measured, its weight is redistributed). TTFB and transferred bytes are reported and produce findings but are not weighted. Lab values can differ from what real visitors experience (field data).

When the site cannot be reached at all, no score is calculated: every category shows "N/A" and the report explains how to make the site reachable.

When coverage is incomplete (time or safety ceilings reached), the four content-based categories are capped between 70 and 92 according to the share of pages analysed.

## Coverage and confidence

Each report carries discovered, eligible, scanned, skipped, blocked and failed URL counts together with the page, depth and time limits. Scores are explicitly based on the analysed sample when coverage is incomplete.

## Paid audit modules (tested checks)

The paid audits add checks that are observed or read from an official source, never inferred. Each module records how it was checked and the raw evidence; a module that cannot run is shown as "not checked" with the reason and produces no finding.

- Search positions come from a paid SERP API (mobile results, located at the business), never from scraping Google.
- The Google Business profile and three nearby businesses of the same primary type come from the Places API (New). Places content is removed from stored reports after 30 days; only place IDs are kept.
- Real-visitor speed is the 75th percentile from the Chrome UX Report (28 days, phone). When Google has no data for the origin the report says so and refers to the lab measurement.
- The measurement test drives a phone-sized browser: no choice, refusal, acceptance, a tap on the phone link and on the WhatsApp link. It records which vendors received data and which tracking cookies were set at each step. Data requests to measurement vendors are answered locally, so test visits and test conversions never reach the owner's statistics.
- The form test sends one message, labelled as an automated test, only with the owner's recorded consent and proof of site ownership. CAPTCHA-protected forms are never submitted. Response time is the delay until someone presses the confirmation button reached from the link in the message.
- Search Console and Analytics are read once with read-only access; the access token is not stored.
- AI assistants are asked customer questions with their own web search enabled; the report shows the answers and whether the business is named.
- Mandatory legal details are searched on the home page and legal pages with country rules (RO, FR, GB, DE, generic EU). This is a technical check, not legal advice. An analyst model may re-check missing items, and its answer counts only when it quotes text that is on the page.
- The ten priorities are ordered by computation (severity, impact, effort; at most three per module). AI opinions are labelled as opinions and never enter the priorities. Reworded text is kept only if it introduces no number that is absent from the finding.
