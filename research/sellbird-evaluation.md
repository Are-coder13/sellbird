# Sellbird evaluation

**Status:** done, 24 November 2026
**Decision:** don't adopt Sellbird. We already run our own outreach tool, built in-house and kept outside this repository, which covers the same workflow.

This fork stays as a reference. Pull ideas from it; don't run it against prospects.

## Why not

1. **Belgian law.** Sellbird is built to find a company's owner and email their personal work address (`first.last@`), falling back to a company inbox only when it finds no person. Belgian law turns that order around. Marketing email without prior consent may go only to impersonal addresses of legal entities (`info@`, `sales@`). An employee's own address is a natural person's address and needs their consent. A sole trader is not a legal entity, so even their `info@` needs consent.
   - Sources: Code of Economic Law art. XII.13; Royal Decree of 4 April 2003 and its report to the King; FOD Economie, "Spam in 23 vragen en antwoorden" (September 2019), questions 8 and 13; Data Protection Authority recommendation 01/2025 on direct marketing.
   - Sellbird's optional compliance check (Precleared) covers 14 countries. Belgium is not one of them.
2. **Language.** The writer has no language setting. Opt-out phrases are matched in English, French, Spanish and Hebrew, not Dutch; Dutch opt-outs depend on the model fallback. Escalation keywords ("lawyer", "GDPR") are English only. The owner finder knows English and French titles, not Dutch ones.
3. **Data.** Our own prospect lists, built from public company registers and annual accounts, are better than Sellbird's discovery sources (OpenStreetMap, DataForSEO, web search; default geography is the United States). Sellbird has no way to import our own list.
4. **Duplication.** Our tool already does what Sellbird does: tracks by offer, a hook per company, a three-email sequence, a do-not-contact list and weekly metrics.

## What Sellbird does well

- The full test suite passed on 24 November 2026: 3,700 passed, 29 skipped, 0 failed (Python 3.13, run on this fork at upstream commit `25ec839`).
- It's actively maintained (last upstream commit 18 November 2026).
- Safety design worth copying: every email waits for approval by default, a reply from anyone at a company stops the sequence to all of them, a bounce-rate kill switch, and recorded opt-outs.
- The license is AGPL-3.0. Internal use is fine; offering a modified version to others as a network service means publishing the changed source.
- Claude plan use: Anthropic's Claude Code legal page allows signing in to the unmodified Claude Code binary with your own subscription, which is what Sellbird calls. The page also says plans are designed for "ordinary use", and an agent that runs every 15 minutes uses that quota.

## Ideas to borrow into our own tool

- An approval outbox: nothing sends without a click.
- Stop the sequence to the whole company on any reply from that company.
- Pause all sending when the bounce rate passes a threshold.
- Rule-based opt-out matching first, with a model fallback. We'd add Dutch phrases ("uitschrijven", "geen interesse", "niet meer mailen").

## Revisit only if

All three are true:
- one outreach track gets positive replies reliably;
- sending by hand has become what's holding us back;
- and Sellbird has gained impersonal-address-only sending, Dutch writing rules and opt-outs, and a CSV import.
