
# RxNova Health OSINT Threat Intelligence Capstone
A defensive, open-source intelligence (OSINT) assessment of RxNova Health, a fictionalized prescription-discount healthcare marketplace. The project applies a structured five-phase CTI intelligence cycle to map the organization's public digital footprint and identify exposure a threat actor could realistically exploit, using only passive, publicly available data.

## Table of Contents
- Project Overview
- Network Topology
- Tools and Technologies
- Configuration Steps
- Results and Findings
- Author

## Project Overview
This capstone project applies OSINT and cyber threat intelligence (CTI) methodology to a real-world-style target, **RxNova Health**, a healthcare technology company operating a prescription price-transparency and discount marketplace. The goal was to identify the organization's publicly exposed digital footprint, subdomains, email infrastructure, and third-party data exposure, and to assess the risk each finding represents from an attacker's perspective, without conducting any active scanning, exploitation, or unauthorized access. The engagement was performed under a signed, passive-only Rules of Engagement and stayed within the bounds of the organization's public bug-bounty program scope.

## Network Topology
Since this was a passive OSINT engagement rather than an internal network assessment, "topology" here refers to the organization's **externally observable digital footprint** — the public-facing structure discovered through domain and subdomain enumeration.

- **Primary domain:** `rxnova.com`, resolving to cloud-hosted infrastructure rather than a self-managed data center.
- **Hosting layer:** Primarily **AWS** (`us-west-2` region), fronted by a **Cloudflare** CDN — confirmed via IP lookups and Shodan organizational queries, which returned zero self-hosted Autonomous System Numbers (ASNs).
- **Subdomain layer:** A passive sweep returned roughly **900 associated hosts**, including:
  - `blocked.rxnova.com`, `swag.rxnova.com`, `click.contact.rxnova.com`, `click.hcp.rxnova.com` — validated clean.
  - `graph.rxnova.com` — clean on direct scan, but flagged in 10+ malware samples on VirusTotal, suggesting it may be referenced (not compromised) by threat actors to blend malicious traffic with legitimate API activity.
- **Authentication endpoints:** `rxnova.com/account/sign-in` (savings program) and `rxnova.com/care/login` (telehealth) — both publicly disclosed in the organization's bug-bounty program scope.
- **Example IP space** (illustrative placeholders, not real addresses): `203.0.113.10`, `203.0.113.44` — both validated as clean, 0% abuse confidence.
- **Email infrastructure:** Publicly discoverable system addresses such as `system@rxnova.com` and `no-reply@rxnova.com`.

## Tools and Technologies
- **Google Dorks** — advanced search-operator queries to surface exposed pages, login clones, and leaked documents
- **theHarvester** (v4.10.1) — passive email, subdomain, and host enumeration
- **Kali Linux** (VirtualBox) — operating environment for reconnaissance tooling
- **VirusTotal** — domain and IP reputation validation against 90+ security vendors
- **AbuseIPDB** — IP abuse-confidence scoring for discovered hosting infrastructure
- **Have I Been Pwned (HIBP)** — breach-exposure checking for organizational email addresses
- **Shodan** — internet-facing infrastructure and exposed-service discovery (observation only)
- **Wayback Machine** — historical web-page analysis for retired infrastructure and cloneable page layouts

## Configuration Steps
1. Defined Priority Intelligence Requirements (PIRs) and mapped each to an appropriate OSINT source (Planning & Direction phase).
2. Ran passive subdomain and email enumeration from Kali Linux using a command such as `theHarvester -d rxnova.com -b all` to sweep public sources.
3. Executed targeted Google Dork queries against the domain to check for lookalike domains, exposed documents, and paste-site leaks.
4. Logged every discovered indicator (emails, subdomains, IPs, URLs) into a structured IOC tracking worksheet with an initial `Unvalidated` status.
5. Validated each subdomain and IP through `VirusTotal` and `AbuseIPDB`, and checked each discovered email address against `Have I Been Pwned`.
6. Cross-referenced organizational and title-based queries in `Shodan` to confirm hosting posture and rule out exposed administrative backends or open databases.
7. Reviewed the `Wayback Machine` archive of the domain to check for retired, still-accessible page layouts that could be repurposed for phishing kits.
8. Compiled all validated indicators, findings, and risk ratings into the final SOC/CTI-ready intelligence report.

## Results and Findings
- **No active phishing or brand impersonation** was identified — no lookalike domains or cloned login pages were found targeting the organization at assessment time.
- **Core infrastructure is clean:** four of five deeply validated subdomains returned zero detections across 90+ VirusTotal vendors; both tested IP addresses returned 0% abuse confidence on AbuseIPDB.
- **One subdomain of interest:** `graph.rxnova.com` returned clean direct scan results but was flagged as appearing in 10+ malware samples, warranting SOC-level monitoring even though the domain itself is not compromised.
- **Unauthorized third-party data scraping:** a commercial scraping tool was found actively extracting the organization's pricing data without authorization, at roughly $60 per 1,000 pages.
- **Third-party breach exposure:** one organizational email address was found in an unrelated third-party data breach (details fictionalized), confirming that organization-associated addresses circulate outside the company's direct control — this does not indicate a breach of the organization itself.
- **Overall risk rating: Medium.** Risk is concentrated in the human/social layer (phishing and brand-impersonation potential) and in third-party data exposure, not in technical infrastructure vulnerabilities.
- **Key recommendations:** continuous brand/domain monitoring, a targeted phishing-awareness campaign for users, investigation of the unauthorized scraper, and enhanced SOC monitoring on `graph.rxnova.com`.

## Author
**Jeannie Cowans** — OSINT / Cyber Threat Intelligence Analyst
*(Add your preferred contact info — email, LinkedIn, or portfolio link — before publishing.)*
