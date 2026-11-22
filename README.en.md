# Incident response on a WordPress website (case study)

[Português](README.md) · **English**

Investigation and clean-up of a corporate WordPress website that had been compromised for about 11 months without anyone noticing. The site served scam pages to Google and stole data typed into its forms. It was fixed the same day, with no downtime.

Done during my work at an environmental consultancy. The incident was on the corporate website (WordPress on shared hosting); the [environmental compliance portal](https://github.com/hnryguchi/case-study-environmental-portal) is a separate system and was not affected. Documentation only: no domains, malicious file names, credentials or infrastructure identifiers.

> The website and hosting belong to the employer. This repository describes the method and the decisions, not the environment. See [What is intentionally omitted](#what-is-intentionally-omitted).

## How it started

In November 2026, the manager reported that searching for the company's name on Google showed the top result under a different name: a fake German-language shop. Anyone who typed the address into a browser saw the normal site, so nobody had noticed.

## My role

I handled the investigation, containment, clean-up and follow-up on my own, with access to the hosting control panel (file manager, database and logs) and to WordPress. I used AI-assisted development (Claude Code) to read the files and cross-check dates. The decisions on what to remove and when were mine.

## Investigation

- **Entry point:** a backdoor (a disguised file manager) hidden inside a theme. Based on file dates, it had been there since **25 Dec 2025**, so the site was compromised for about 11 months.
- **Cloaking:** scam kits published in subfolders answered differently depending on the visitor. Google's crawler and visitors coming from ads saw the fake shop; direct visitors saw the real site. That is why nobody at the company saw the problem. The ads were tied to a scam operation abroad.
- **Data theft:** a loader hidden in the WordPress core contacted an external server and **sent out whatever was typed into the site's forms**.
- **Re-infection:** a second installer file had been planted on 31 Oct 2026, a sign that access was still active.

The method was to rebuild the timeline from file modification dates, compare the WordPress core against the official release, and check every plugin and theme: is it in use? was it modified? where did it come from?

## Containment and clean-up

- **Quarantine instead of delete.** Everything compromised or unused was moved to a folder outside the public area: 22 plugins, 8 themes, old subdomains and an old `wp-config`. The site kept working without them, and the evidence was preserved.
- **Rotate every secret.** Database password, WordPress keys and salts, and admin passwords.
- **Hardening:** file editing from the dashboard disabled, `xmlrpc` blocked, PHP execution blocked in the uploads folder, and automatic updates turned on.
- **Google reputation:** verified the domain in Google Search Console (via DNS), requested a review of the "deceptive pages" warning, and requested re-indexing.
- **Monitoring:** set up a weekly check (every Monday at 09:00) that compares the site's files with a clean baseline and alerts on any change.

## Outcome

- Site clean the same day, with **no downtime**.
- Form data leak stopped; attacker access cut off by rotating the secrets.
- Google review requested and continuous monitoring in place.

## Decisions

- **Keep the site online.** The real site worked and the scam lived in separate files, so it could be cleaned live without affecting visitors.
- **Quarantine before deleting.** Keeping the files allows later investigation and undoing a wrong removal. Permanent deletion is scheduled for December 2026, after a period with no re-infection.
- **Assume everything leaked.** With an active loader present, every secret was rotated, not only the ones that looked exposed.

## Limitations

- It was not possible to tell for certain which plugin brought in the malicious code; most likely one of those now in quarantine.
- The entry date comes from file dates, which an attacker can alter. It is an estimate, not proof.

## Lessons

- An unmaintained site becomes a reputation risk and a data-leak point, even if it is "just the corporate website".
- Cloaking hides the problem from the site's own owners. It is worth searching for the company's name on Google now and then, not just opening the address.
- A simple routine covers most of it: automatic updates, removing what is unused, reviewing who has access, and monitoring file changes.

## What is intentionally omitted

- **Domain, hosting and file names.** No portfolio value; they would only help someone attack again.
- **Indicators of compromise** (external server addresses, malicious file names and hashes).
- **Credentials and configuration**, old or new.

---

> © Henry Noguchi. **All rights reserved.** Public read-only repository; copying, reuse or presenting it as one's own requires permission. See [LICENSE](LICENSE).
