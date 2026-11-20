# Ley 21.719 Compliance Auditor

Desarrollado por [Zythos Media](https://zythos.media) — Especialistas en SEO & IA Search

**Audit any Chilean website for Ley 21.719 compliance directly inside Claude Code.**

Chile's Personal Data Protection Law (Ley 21.719, published December 2024) replaces
the previous Ley 19.628 and introduces GDPR-like obligations: explicit consent, seven
data subject rights (including portability, restriction, and protection from automated
decisions), mandatory privacy policy content, international transfer rules, breach
reporting and a voluntary data protection officer. Under the published text it enters into
force on 1 December 2026; a postponement to 15 October 2027 (Boletín 18.623-07) was
dispatched by Congress on 17 November 2026 and becomes law only when published in the
Diario Oficial.

This Claude Code skill inspects a website externally — no backend access required —
and produces a scored compliance report with actionable remediation steps and
**sanction-level risk per issue**.

---

## What it audits

10 compliance blocks mapped to Ley 21.719:

| Block | Topic | Notes |
|-------|-------|-------|
| 1 | Data controller identification | Art. 14 ter. Controllers without a Chilean domicile must keep an operative email or contact channel (no local representative required) |
| 2 | Privacy policy — existence and access | Art. 14 ter |
| 3 | Privacy policy — minimum required content | Art. 14 ter (duty of information and transparency) |
| 4 | Legal basis declared per processing purpose | Arts. 12-13. Six valid bases under Chilean law |
| 5 | Cookie consent, opt-in and **marketing direct opt-out** | Art. 12 |
| 6 | **Seven** data subject rights channel (ARCOP+L+A) | Incl. right against automated decisions; deadlines in Art. 11 |
| 7 | Sensitive data handling | Art. 2 g) + 16. Children's data has its own regime (Art. 16 quáter, under 14) |
| 8 | International data transfers with guarantees | Arts. 27-29. Habitual flows need guarantees; consent only for specific, non-habitual transfers |
| 9 | Security and breach reporting | Arts. 14 quinquies and 14 sexies |
| 10 | Data protection officer — voluntary | Art. 50 (part of the prevention model, Arts. 49-52) |

**Automatic sector detection:** the skill detects the site's vertical (health,
e-commerce, fintech, education, SaaS, media, professional services) and applies
sector-specific requirements — stricter rules for health data, minor-consent logic
for education sites, CMF regulatory overlay for fintech, etc.

---

## Score

Each block is weighted. The total score is 0–100:

| Range | Risk posture |
|-------|-------------|
| 80–100 | Defensible — substantial compliance |
| 60–79 | Moderate risk — important gaps, no critical blockers |
| 40–59 | High risk — multiple material non-compliances |
| 0–39 | Critical risk — essential elements missing |

---

## Sanctions

Every issue in the report includes the applicable sanction level:

| Level | Maximum fine | Examples |
|-------|-------------|---------|
| Gravísima | Up to 20,000 UTM | Sensitive data without consent; international transfer without guarantees |
| Grave | Up to 10,000 UTM | No privacy policy; rights channel missing; no declared legal basis |
| Leve | Written warning or up to 5,000 UTM | Incomplete policy; no footer link; retention periods unspecified |

Fines per Art. 35. Recidivism (two or more sanctions within 30 months, Art. 36): up to 3×
the fine and, for companies that are not small businesses, up to 2% (grave) or 4%
(gravísima) of annual revenue. UTM = Chilean monthly tax unit: convert to CLP with the
current month's value.

---

## Install

Copy `SKILL.md` into your Claude Code skills directory:

**macOS / Linux**
```bash
mkdir -p ~/.claude/skills/ley-datos-chile
curl -o ~/.claude/skills/ley-datos-chile/SKILL.md \
  https://raw.githubusercontent.com/carlosuhart/ley-datos-chile/main/SKILL.md
```

**Windows (PowerShell)**
```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\skills\ley-datos-chile"
Invoke-WebRequest `
  -Uri "https://raw.githubusercontent.com/carlosuhart/ley-datos-chile/main/SKILL.md" `
  -OutFile "$env:USERPROFILE\.claude\skills\ley-datos-chile\SKILL.md"
```

The skill is available in Claude Code at the start of the next session.

---

## Usage

```
# Markdown report (internal use)
/ley-datos-chile https://ejemplo.cl

# Word document with executive summary and actionables
/ley-datos-chile https://ejemplo.cl --docx

# GDPR comparison column (only when explicitly requested)
/ley-datos-chile https://ejemplo.cl --rgpd
```

The skill always asks before generating any document: internal use (Markdown) or
client-facing (Word with executive language and prioritized next steps).

---

## What you get

Every issue — critical, medium, or low — includes evidence, a concrete action, and sanction risk:

```
**[Issue name]** (Block X — Ley 21.719)
Evidence: [what was found or what is missing]
Sanction risk: [Infracción grave/gravísima — up to X UTM]
→ Action: [concrete remediation step]
→ Suggested timeline: [30 / 60 days]   ← medium issues only
```

---

## Stack integration

### With seo-audit (AgriciDaniel/claude-seo)

When `/seo audit` detects a Chilean site (`.cl` TLD, RUT/SII/CMF/SERNAC signals,
CLP currency), it spawns `ley-datos-chile` as a conditional subagent and folds the
compliance score into the unified SEO health report.

### With anonimizar (carlosuhart/anonimizador-datos)

Audit reports may contain the audited site's PII (responsible party name, email,
RUT, data protection officer contact). Before sharing externally:

```
/anonimizar auditoria_ley21719_2026-06-28.md --ley chile
/anonimizar auditoria_ley21719_2026-06-28.docx --ley chile
```

Use `--ley chile` (not `--ley todo`) to avoid false positives on article numbers
(Art. 12, Art. 27, etc.).

---

## Audit scope

This skill audits what is observable externally without backend access:

- Public site documents (privacy policy, terms)
- Cookie and consent banner behavior
- Form consent mechanisms and marketing opt-out
- HTTP headers (HTTPS, HSTS, CSP, X-Frame-Options)

**Out of scope** (requires internal access):

- Data processor agreements (Art. 15 bis)
- Records of processing activities
- Internal rights-request response procedures
- Internal security measures (encryption at rest, access controls)
- Breach reports to the Agency (Art. 14 sexies)

The report always includes a methodology note distinguishing what was verified
externally from what remains unverifiable without backend access.

---

## Normative note

Ley 21.719 was published 13 December 2024. Under the published text it enters into force
on 1 December 2026 (first transitory article). Boletín 18.623-07, dispatched by Congress on
17 November 2026, moves that date to 15 October 2027 without changing obligations or
fines; until it is published in the Diario Oficial, the legal date remains 1 December 2026.
Check diariooficial.interior.gob.cl before quoting a date.

**Control authority:** Ley 21.719 creates the **Agencia de Protección de Datos
Personales**, which starts operating when the law enters into force. The Consejo para la
Transparencia does **not** exercise its functions at any point: Art. 31 only regulates
coordination between the two bodies.

**Implementing regulations (*reglamentos*):** not yet published as of 20 November 2026.
They may refine third-country adequacy, certification and the prevention model.

All article references were verified against the official BCN text (idNorma 1209272) on
20 November 2026.

---

## Changelog

### v1.3.0 — 2026-11-20
- **Articles verified against the official BCN text:** information duty (14 ter), security
  (14 quinquies), breach reporting (14 sexies, new check), processor (15 bis), transfers
  (27-29), sanctions and recidivism (35-36), data protection officer (50, voluntary)
- **Authority corrected:** the CPLT never exercises the Agency's functions (Art. 31)
- **No local representative required** for controllers without a Chilean domicile: an
  operative contact channel is enough
- **Sensitive data** per Art. 2 g) (adds human biological profile and gender identity);
  children's data has its own regime, not a sensitive category
- **Fines** without undated USD equivalents; written warning for minor infringements
- **Entry into force:** 1 December 2026 per the published text, with the status of the
  postponement bill (Boletín 18.623-07)

### v1.2.2 — 2026-10-01
- Response deadline for data subject requests corrected to 30 calendar days, extendable
  once by 30, and temporary blocking within 2 business days (Art. 11)

### v1.2.1 — 2026-07-03
- **Credit line added:** Zythos Media credit emitted at the start of each audit run
- Markdown template and .docx footer updated

### v1.2.0 — 2026-06-28
- **Block 0 added:** automatic stack detection (CMS, page builder, CDN, server, plugins, third-party scripts)

### v1.1.0 — 2026-06-28
- **Authority corrected:** CPLT replaced by APDP throughout, with transition note
- **Seventh data subject right added:** right not to be subject to decisions based solely on automated processing (omitted in v1.0.0)
- **Sensitive data categories completed:** socioeconomic status, ideological/philosophical convictions, sexual life, and professional affiliations added
- **Six legal bases:** added economic/financial obligations and legal defense as valid bases (Chilean law specific, absent in v1.0.0)
- **Marketing direct opt-out:** new verifiable sub-block in Block 5
- **Sanctions table:** every issue now includes the applicable fine level in UTM
- **EPD criteria corrected:** removed GDPR Art. 37 criteria; replaced with Chilean law framework
- **International transfers:** BCR and certification mechanisms added as valid guarantees
- **Security attributes:** confidentiality, integrity, availability, resilience explicitly required in policy check
- **Article references marked ⚠️** where BCN text was inaccessible for direct verification

### v1.0.0 — 2026-06-28
- Initial release

---

## License

MIT — see [LICENSE](LICENSE).

---

## Related tools

- [carlosuhart/anonimizador-datos](https://github.com/carlosuhart/anonimizador-datos) — multi-jurisdiction PII anonymizer (RGPD, Ley 21.719, LGPD, LFPDPPP, Ley 1581, CCPA)
- [AgriciDaniel/claude-seo](https://github.com/AgriciDaniel/claude-seo) — full SEO audit suite for Claude Code
