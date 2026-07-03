# Ley 21.719 Compliance Auditor

Desarrollado por [Zythos Media](https://zythos.media) — Especialistas en SEO & IA Search


**Audit any  Chilean website for Ley 21.719 compliance dir ectly inside Claude Code.**

Chile's Personal  Data Protection Law (Ley 21.719, published D ecember 2024) replaces
the previous Ley 19.62 8 and introduces GDPR-like obligations: expli cit consent, seven
data subject rights (inclu ding portability, restriction, and protection  from automated
decisions), mandatory privacy  policy content, international transfer rules , and a Data
Protection Officer (EPD) require ment in certain cases. The 24-month adaptatio n period
ends December 2026.

This Claude Cod e skill inspects a website externally — no  backend access required —
and produces a sc ored compliance report with actionable remedi ation steps and
**sanction-level risk per iss ue**.

---

## What it audits

10 compliance  blocks mapped to Ley 21.719:

| Block | Topic  | Notes |
|-------|-------|-------|
| 1 | Da ta controller identification | Art. 5. Foreig n entities must designate a Chilean represent ative |
| 2 | Privacy policy — existence an d access | Art. 14 |
| 3 | Privacy policy —  minimum required content | Art. 14 a–k + p ublic display obligation |
| 4 | Legal basis  declared per processing purpose | Art. 12. Si x valid bases under Chilean law |
| 5 | Cooki e consent, opt-in and **marketing direct opt- out** | Art. 12 |
| 6 | **Seven** data subjec t rights channel (ARCOP+L+A) | Incl. right ag ainst automated decisions |
| 7 | Sensitive d ata handling — **9 categories** | Art. 2 +  16. Socioecon. status, ideology, sexual life  added in v1.1 |
| 8 | International data tran sfers with guarantees | Art. 26. BCR and cert ification mechanisms included |
| 9 | Technic al security — confidentiality, integrity, a vailability, resilience | Art. 19 |
| 10 | Da ta Protection Officer (EPD) — applicability  under Chilean law | Art. 36 |

**Automatic s ector detection:** the skill detects the site 's vertical (health,
e-commerce, fintech, edu cation, SaaS, media, professional services) a nd applies
sector-specific requirements — s tricter rules for health data, minor-consent  logic
for education sites, CMF regulatory ove rlay for fintech, etc.

---

## Score

Each b lock is weighted. The total score is 0–100: 

| Range | Risk posture |
|-------|--------- ----|
| 80–100 | Defensible — substantial  compliance |
| 60–79 | Moderate risk — i mportant gaps, no critical blockers |
| 40– 59 | High risk — multiple material non-comp liances |
| 0–39 | Critical risk — essent ial elements missing |

---

## Sanctions

Ev ery issue in the report includes the applicab le sanction level:

| Level | Maximum fine |  Examples |
|-------|-------------|---------|
 | Gravísima | 20,000 UTM (~USD 1,590,000) or  2–4% annual revenue | Sensitive data witho ut consent; international transfer without gu arantees |
| Grave | 10,000 UTM (~USD 795,000 ) | No privacy policy; rights channel missing ; no declared legal basis |
| Leve | 5,000 UT M (~USD 397,500) | Incomplete policy; no foot er link; retention periods unspecified |

Rec idivism: up to 3× the base fine. UTM = Chile an monthly tax unit (indexed monthly).

---

 ## Install

Copy `SKILL.md` into your Claude  Code skills directory:

**macOS / Linux**
``` bash
mkdir -p ~/.claude/skills/ley-datos-chil e
curl -o ~/.claude/skills/ley-datos-chile/SK ILL.md \
  https://raw.githubusercontent.com/ carlosuhart/ley-datos-chile/main/SKILL.md
``` 

**Windows (PowerShell)**
```powershell
New- Item -ItemType Directory -Force "$env:USERPRO FILE\.claude\skills\ley-datos-chile"
Invoke-W ebRequest `
  -Uri "https://raw.githubusercon tent.com/carlosuhart/ley-datos-chile/main/SKI LL.md" `
  -OutFile "$env:USERPROFILE\.claude \skills\ley-datos-chile\SKILL.md"
```

The sk ill is available in Claude Code at the start  of the next session.

---

## Usage

```
# Ma rkdown report (internal use)
/ley-datos-chile  https://ejemplo.cl

# Word document with exe cutive summary and actionables
/ley-datos-chi le https://ejemplo.cl --docx

# GDPR comparis on column (only when explicitly requested)
/l ey-datos-chile https://ejemplo.cl --rgpd
```
 
The skill always asks before generating any  document: internal use (Markdown) or
client-f acing (Word with executive language and prior itized next steps).

---

## What you get

Ev ery issue — critical, medium, or low — in cludes evidence, a concrete action, and sanct ion risk:

```
**[Issue name]** (Block X —  Ley 21.719)
Evidence: [what was found or what  is missing]
Sanction risk: [Infracción grav e/gravísima — up to X UTM]
→ Action: [co ncrete remediation step]
→ Suggested timeli ne: [30 / 60 days]   ← medium issues only
` ``

---

## Stack integration

### With seo-a udit (AgriciDaniel/claude-seo)

When `/seo au dit` detects a Chilean site (`.cl` TLD, RUT/S II/CMF/SERNAC signals,
CLP currency), it spaw ns `ley-datos-chile` as a conditional subagen t and folds the
compliance score into the uni fied SEO health report.

### With anonimizar  (carlosuhart/anonimizador-datos)

Audit repor ts may contain the audited site's PII (respon sible party name, email,
RUT, EPD contact). B efore sharing externally:

```
/anonimizar au ditoria_ley21719_2026-06-28.md --ley chile
/a nonimizar auditoria_ley21719_2026-06-28.docx  --ley chile
```

Use `--ley chile` (not `--le y todo`) to avoid false positives on article  numbers
(Art. 12, Art. 26, etc.).

---

## Au dit scope

This skill audits what is observab le externally without backend access:

- Publ ic site documents (privacy policy, terms)
- C ookie and consent banner behavior
- Form cons ent mechanisms and marketing opt-out
- HTTP h eaders (HTTPS, HSTS, CSP, X-Frame-Options)

* *Out of scope** (requires internal access):

 - Data processor agreements (Art. 24)
- Recor ds of processing activities (Art. 15)
- Inter nal rights-request response procedures
- Inte rnal security measures (encryption at rest, a ccess controls)
- Breach notifications to the  APDP

The report always includes a methodolo gy note distinguishing what was verified
exte rnally from what remains unverifiable without  backend access.

---

## Normative note

Ley  21.719 was published 13 December 2024. The 2 4-month adaptation period for most
obligation s ends December 2026.

**Control authority:**  Ley 21.719 creates the **Agencia de Protecci ón de Datos
Personales (APDP)** as the super visory authority. During the transition perio d until
the APDP is operational (estimated 20 26–2027), the **Consejo para la Transparenc ia
(CPLT)** exercises its functions.

**Imple menting regulation (*reglamento*):** still in  drafting as of v1.1.0 — some
implementatio n details (age threshold for minors, EPD appl icability criteria, third-country
adequacy de cisions) may be refined upon publication. The  audit flags this where relevant.

---

## Ch angelog

### v1.1.0 — 2026-06-28
- **Author ity corrected:** CPLT replaced by APDP throug hout, with transition note
- **Seventh data s ubject right added:** right not to be subject  to decisions based solely on automated proce ssing (omitted in v1.0.0)
- **Sensitive data  categories completed:** socioeconomic status,  ideological/philosophical convictions, sexua l life, and professional affiliations added
-  **Six legal bases:** added economic/financia l obligations and legal defense as valid base s (Chilean law specific, absent in v1.0.0)
-  **Marketing direct opt-out:** new verifiable  sub-block in Block 5
- **Sanctions table:** e very issue now includes the applicable fine l evel in UTM
- **EPD criteria corrected:** rem oved GDPR Art. 37 criteria; replaced with Chi lean law framework
- **International transfer s:** BCR and certification mechanisms added a s valid guarantees
- **Security attributes:**  confidentiality, integrity, availability, re silience explicitly required in policy check
 - **Article references marked ⚠️** where  BCN text was inaccessible for direct verifica tion

### v1.0.0 — 2026-06-28
- Initial rel ease

---

## License

MIT — see [LICENSE]( LICENSE).

---

## Related tools

- [carlosuh art/anonimizador-datos](https://github.com/ca rlosuhart/anonimizador-datos) — multi-juris diction PII anonymizer (RGPD, Ley 21.719, LGP D, LFPDPPP, Ley 1581, CCPA)
- [AgriciDaniel/c laude-seo](https://github.com/AgriciDaniel/cl aude-seo) — full SEO audit suite for Claude  Code
 