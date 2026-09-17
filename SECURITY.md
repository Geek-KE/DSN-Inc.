# Mtaa Pulse

**Community Insight, Verified at the Source.**

Mtaa Pulse is a ground-truth reporting platform. It lets ordinary residents, the people who actually live and work in a place, record the real, present-day condition of their environment: roads, security, soil and crops, water and drainage, minerals, infrastructure, or anything else worth flagging. Not opinions, not rumor, not headline narratives. What's actually there, backed by a photo or video and a location that the reporter cannot fake.

That verified, ground-level record is then made available to the people whose decisions and research should be shaped by reality rather than assumption: **students** looking for research questions that matter, and **politicians and investors** who need to know what's actually happening in a place before they promise or fund anything.

---

## The problem this is trying to solve

Kenya (and much of the world) runs on secondhand narratives. A region gets labeled "insecure" or "unproductive" once, and that label outlives the reality on the ground for decades, even while people are living, farming, trading, and innovating there every day. Meanwhile:

- Universities keep producing graduates, but it's fair to ask whether their research is actually pointed at the problems that matter to the people affected by them.
- Politicians cycle through election after election, often campaigning on the same recycled, second-hand version of what a place needs.
- Investors and companies form and dissolve without ever getting an accurate read on where the real opportunity or the real gap is.

Living standards stay flat not necessarily because no one is working, but because the work is often aimed at the wrong problem, informed by the wrong information.

Mtaa Pulse exists to close that gap: **let the people on the ground report the ground truth, and put that truth directly in front of the people who research, legislate, and invest.**

---

## Who it's for

| Group | What they do with it |
|---|---|
| **Residents (reporters)** | Submit real-time, evidence-backed reports about their area (positive or negative), anonymously if they choose. |
| **University & college students** (diploma, degree, master's, doctoral) | Browse verified reports to find real, under-researched problems worth studying, instead of picking topics based on assumption or convenience. |
| **Politicians** | Access verified, area-specific reports to understand actual constituent conditions and build manifestos around real needs, not guesswork. |
| **Investors & companies** | Identify real gaps, real resources, and real demand on the ground before committing capital. |

---

## Core principles

1. **Report reality, not just problems.** A report can be negative (bad roads, insecurity, infertile soil) or positive (well-managed drainage, strong security, fertile land, undocumented mineral deposits). The point is accuracy, not negativity.
2. **Every report must be backed by evidence.** A report isn't published on claims alone: it requires a photo (and, depending on the flow, a short video) captured at the time of reporting, plus a device-captured location.
3. **Anonymity for the reporter, accountability behind the scenes.** A resident can choose to appear anonymous (pseudonym) on the public feed. But every report still collects identifying information on the backend, including a selfie, so the platform can stand behind the authenticity of what's published. Reporters are told plainly that this identifying data is kept private and is not exposed publicly.
4. **Verification before publication.** Nothing goes live automatically. Every submitted report enters a `pending` state and passes through admin review before it appears on the public feed.
5. **Location has to be real.** Reports are location-locked to where the reporter says they are: see "Location Integrity" below for exactly what's enforced today and what's on the roadmap.

---

## How a report gets from a resident's phone to the feed

1. **Start a report**: pick a category (roads, security, agriculture, water/drainage, minerals, etc.), county, and write the description.
2. **Capture location**: the browser's geolocation API captures the device's real-time coordinates. These are reverse-geocoded server-side and checked against the county the reporter selected. If they don't match, the reporter is stopped and asked to recapture from the correct site; the mismatch is never silently accepted.
3. **Capture photo / video**: evidence is captured directly through the device camera as part of the flow (not uploaded from an arbitrary gallery file), and stored in dedicated photo/video storage buckets.
4. **Verify identity (email OTP)**: the reporter verifies their email with a one-time code before the report can be submitted, and, depending on flow, provides basic identifying details privately for the backend.
5. **Choose visibility**: the reporter chooses a public pseudonym; their real identity is never shown on the public feed.
6. **Submit**: the report is written to the database as `verified: false` (pending) along with its coordinates, accuracy, evidence URLs, and metadata. The reporter sees a clear on-screen confirmation once submission succeeds.
7. **Admin review**: an authenticated admin (also OTP-gated) reviews pending reports and approves or rejects them before they appear publicly.
8. **Publish**: approved reports become part of the public, browsable ground-truth feed.

---

## Location integrity

**Enforced today:**
- Coordinates are captured directly from the device via the browser Geolocation API at the moment of reporting, not typed in manually.
- Captured coordinates are reverse-geocoded and cross-checked against the county the reporter selected. A mismatch blocks submission until the reporter recaptures at the correct location.
- Raw coordinates and the internal match/mismatch detail are used for backend verification only and are never exposed on the public feed.

**On the roadmap (not yet implemented, noted here so it isn't overstated):**
- Automated detection and blocking of VPNs, proxies, and other IP-masking activity at login/submission time.
- Device- and sensor-level signals (e.g. mock-location detection, GPS spoofing app detection) to catch manipulated coordinates that pass a simple county check.
- Anomaly detection across a reporter's submission history (e.g. impossible travel between reports).

Anyone contributing to this project on the anti-spoofing side should treat this as the current priority gap between "location is captured" and "location is provably unmanipulated."

---

## Access for researchers, politicians, and investors

Beyond the public feed, the platform supports a **data request flow** for anyone who needs a structured or bulk view of the ground-truth data: a student scoping a thesis, a politician's office prepping a manifesto, an investor doing due diligence. Requesters submit what they need (topics, area, time period, delivery format), verify via OTP, and are granted access; if the primary request pipeline is unavailable, the request still reaches the team by email so nothing is lost.

---

## Tech stack

- **Frontend:** Single-page HTML/CSS/JS app (no framework build step required).
- **Backend / data:** [Supabase](https://supabase.com): Postgres database, Storage (for photo/video evidence), and Edge Functions (for OTP send/verify and other server-side checks).
- **Auth / verification:** Email-based one-time codes for reporters, data requesters, and admins.
- **Maps / geocoding:** Client-side geolocation with server-side reverse-geocoding for the county-match check.

---

## Project status

This is an early-stage build. The reporting flow (capture, verify, submit, review, publish) and the data-request flow for external access are functional. Location spoofing prevention is currently limited to the county cross-check described above; true VPN/proxy/mock-location detection is planned but not yet built. Treat any claim of "spoof-proof" location as aspirational until that work lands.

---

## Vision

If students research the problems that are actually holding people back, if politicians campaign on needs that are actually verified, and if investors put capital where the real gaps and real resources are, instead of all three groups working off decades-old assumptions, the feedback loop between *what's true on the ground* and *what gets funded, studied, and legislated* finally closes. That's what Mtaa Pulse is trying to build toward.
