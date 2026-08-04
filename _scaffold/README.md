# Quarantined scaffold artifacts — MEDITECH

Moved here 2026-07-27 during a provenance review. **These files are not part of the live profile.**

`meditech-plans-pricing.yml`, `meditech-rate-limits.yml`, and `meditech-finops.yml` were generated
by the 2026-05-04 scaffold pass. They assert things MEDITECH does not publish:

- A "Free" plan tier with a 1,000 requests/month quota at `$0.00`
- A 10 requests/minute free-tier rate limit
- `X-RateLimit-Limit` / `X-RateLimit-Remaining` / `X-RateLimit-Reset` response headers
- Billing domains at `https://api.meditech.example.com` (a placeholder host that does not exist)

None of that is evidenced. MEDITECH publishes **no** API pricing, **no** plan tiers, and **no**
rate-limit documentation on any public surface. Its API documentation sits behind the
Greenfield Workspace login at `greenfield.meditech.com`, and both `greenfield.meditech.com` and
`fhir.meditech.com` serve `robots.txt` with `Disallow: /`.

Per the network convention — record what a provider actually publishes and skip what is honestly
absent — the correct state for MEDITECH is **absent**, not scaffolded. The scaffold values were
also feeding `accessModel` (which had derived "Freemium · Self-serve signup" at `confidence: high`)
and inflating the commercial-clarity facet of the Kin Score.

Kept rather than deleted so the scaffold pass that produced them can be traced and the same
pattern found in other repos. Do not restore without provider-published evidence.
