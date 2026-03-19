# Electrical Guru — Claude Code Plugin

Expert NEC 2023 Electrical Installations Consultant for Denver Metro Area and Douglas County, Colorado.

## What It Does

Transforms Claude into a professional-grade electrical code analyst that delivers:

- **Authority-backed code interpretations** — Every answer cites NEC 2023 articles with full section references and is validated by at least two recognized industry authorities (Mike Holt, NFPA Handbook, IAEI, etc.)
- **Structured options analysis** — Every viable installation approach analyzed with code basis, pros/cons, cost implications, inspection considerations, and long-term impact
- **Adversarial self-validation** — Every response undergoes a Devil's Advocate review before delivery, as if being presented to NEC Code-Making Panel members
- **Local jurisdiction awareness** — Colorado state amendments, Denver Metro requirements, and Douglas County requirements are always considered
- **Mandatory disclaimers** — Professional liability protection on every response

## Components

| Type | Name | Purpose |
|------|------|---------|
| Skill | `electrical-guru` | Core behavioral protocol — auto-activates on any electrical/NEC question |
| Command | `/nec` | Explicit activation — type `/nec [question]` to engage guru mode |

## Usage

### Auto-activation (Skill)
Just ask an electrical question. The skill triggers automatically when it detects NEC code, electrical installation, wiring, panel, grounding, or related topics.

### Explicit activation (Command)
```
/nec What are the GFCI requirements for a kitchen remodel in Douglas County?
```

Or just `/nec` to enter consultation mode without a specific question.

## Installation

### Local testing
```bash
claude --plugin-dir ~/GitHub/electrical-guru
```

### Global installation
Add to `~/.claude/settings.json`:
```json
{
  "plugins": [
    "~/GitHub/electrical-guru"
  ]
}
```

## Jurisdiction

- **Primary Code:** NEC 2023 (NFPA 70-2023)
- **Geographic Scope:** Denver Metro Area, Douglas County, Colorado
- **Local Amendments:** Colorado State Electrical Board, City/County of Denver, Douglas County

## Author

Shane Warehime — Phoenix Electric
