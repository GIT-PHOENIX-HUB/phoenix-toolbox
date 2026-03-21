# Volt Marketing — Claude Code Plugin

**Elite Marketing Strategist & Customer Acquisition Specialist for Phoenix Electric CO**

Built by Shane Warehime and Claude for the Phoenix AI system — a persistent, multi-agent electrical contracting operations platform.

---

## What It Does

Transforms Claude into **"Volt"** — a battle-hardened, ROI-obsessed marketing strategist who specializes exclusively in residential electrical contractor marketing for the Colorado Front Range. This is not a generic marketing assistant. Volt thinks like a CMO who also understands the trades.

### Capabilities
- **Full campaign planning** across all 8 service lines (Generac, custom homes, remodels, service calls, basements, renovations, multifamily, EV chargers)
- **Google Ads / LSA / GBP optimization** with exact targeting parameters and negative keywords
- **Generac storm-chasing protocols** — ready-to-activate ad campaigns within hours of outage events
- **Facebook/Instagram campaigns** with demographic and geographic targeting
- **SEO & content strategy** for local search domination across the service territory
- **3-tier budget allocation** (Starter / Growth / Domination) with expected ROI projections
- **Seasonal playbooks** mapped to Colorado weather and market cycles
- **Builder/realtor partnership strategies** for custom home and remodel pipelines
- **Review generation systems** for Google Business Profile dominance
- **ROI tracking and attribution frameworks** — if you can't measure it, don't spend on it

---

## Directory Structure

```
plugins/volt-marketing/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest (name, version, metadata)
├── commands/
│   └── volt.md              # /volt slash command activation
├── skills/
│   └── volt-marketing/
│       └── SKILL.md         # Full skill definition with auto-trigger
├── mcp-server/
│   └── volt-marketing-server.js  # MCP server with 8 marketing tools
├── RUNBOOK.md               # Step-by-step deployment guide
├── PLAYBOOK.md              # Usage scenarios and strategy workflows
└── README.md                # This file
```

---

## Three Ways to Use Volt

### 1. As a Skill (Auto-Triggers)
The SKILL.md file auto-activates when Claude detects marketing-related queries. Just ask about marketing, advertising, lead generation, Google Ads, Generac marketing, etc.

### 2. As a Command (/volt)
Type `/volt` in Claude Code to manually activate Volt with an optional query:
```
/volt What's our best strategy for Generac marketing this spring?
/volt Give me a Facebook ad campaign for kitchen remodels in Castle Rock
/volt Create a Q2 budget allocation for $3,000/month
```

### 3. As an MCP Server
The MCP server exposes 8 tools for programmatic access:
- `volt_campaign_plan` — Full campaign plans by service line
- `volt_budget_allocator` — 3-tier budget recommendations
- `volt_ad_copy` — Platform-specific ad copy generation
- `volt_seasonal_check` — Current seasonal priorities
- `volt_territory_check` — Service territory validation
- `volt_roi_calculator` — ROI estimates by channel/spend
- `volt_review_strategy` — Review acquisition plans
- `volt_storm_protocol` — Storm-response Generac activation

---

## Service Territory

**Primary (75% of service work):**
South Metro Denver, Elizabeth/Elbert County, Castle Rock, Parker, Castle Pines, Lone Tree, Highlands Ranch, Centennial, Littleton, Englewood, Monument, Palmer Lake, Larkspur, Kiowa, Franktown, Sedalia, North Colorado Springs, east to Agate, west to Golden/Morrison/Evergreen (south of I-70 only)

**EXCLUDED (never advertise):**
Winter Park, Silverthorne, Summit County, Grand County, Western Slope

---

## Service Lines by Priority

| # | Service Line | Avg Value | Margin |
|---|---|---|---|
| 1 | Generac Generators | $12K-$25K+ | Highest |
| 2 | Custom Home Electrical | $15K-$80K+ | High |
| 3 | Kitchen/Bath Remodels | $3K-$15K | Good |
| 4 | Service Calls | $250-$2,500 | Moderate |
| 5 | Basement Finishes | $3K-$12K | Good |
| 6 | Whole-Home Renovations | $8K-$30K+ | High |
| 7 | Multifamily/Commercial | Varies | Moderate |
| 8 | EV Chargers/Specialty | $800-$2,500 | Moderate |

---

## Quick Start

See **RUNBOOK.md** for detailed deployment instructions.
See **PLAYBOOK.md** for usage scenarios and strategy workflows.
