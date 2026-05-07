# Salesforce Maps — Agent Skills

[![Agent Skills](https://img.shields.io/badge/Agent_Skills-compatible-0F766E)](https://agentskills.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Small add-on skill library for **Salesforce Maps** (formerly MapAnything): territory management, field sales optimization, setup guidance, and developer API patterns. Use alongside **[forcedotcom/afv-library](https://github.com/forcedotcom/afv-library)** for Apex, Flow, Lightning Web Components (LWC), OmniStudio Common Core, deploy, and testing.

All skills use the **`sf-maps-*`** prefix so IDs stay distinct from upstream `sf-*` skills.

**Start here:** [Available skills](#available-skills) · [Agents](#agents) · [Documentation](#documentation) · [Installation](#installation)

---

## Available skills

| Skill | Purpose |
|-------|---------|
| [sf-maps-setup](skills/sf-maps-setup/) | Installation prerequisites, permission sets, org configuration, and geocoding setup. |
| [sf-maps-territory](skills/sf-maps-territory/) | Territory management, ATM (Advanced Territory Management), coverage rules, and assignment logic. |
| [sf-maps-field](skills/sf-maps-field/) | Field sales workflows — routing, markers, data layers, visit planning, and mobile usage. |
| [sf-maps-custom-actions](skills/sf-maps-custom-actions/) | Map It buttons, Popup Flow actions, and multi-record custom actions in Salesforce Maps. |
| [sf-maps-arcgis](skills/sf-maps-arcgis/) | ArcGIS Living Atlas layer consumption and ArcGIS Connectors / Batch Push to ArcGIS Online or Enterprise. |

Each folder contains a `SKILL.md` plus optional `references/` (see [sf-maps-field/references/](skills/sf-maps-field/references/)).

---

## Documentation

When **`docs/`** is published alongside this pack, open these paths at the **repository root**:

| Path | Purpose |
|------|---------|
| `docs/adr-maps.md` | Maps ADR covering data model decisions, ATM vs. standard territories, and geocoding considerations. |
| `docs/guide-agent-skills.md` | Layered install guide. |
| `docs/sf-maps-legacy-patterns.md` | Legacy MapAnything patterns: Map It buttons (`sma` namespace), popup flows, mass actions, and data layer comparison. |

---

## Agents

| Agent | Purpose |
|-------|---------|
| [sf-maps-architect](agents/sf-maps-architect.md) | Maps technical architect persona; loads all `sf-maps-*` skills; delegates Apex, Flow, and LWC mechanics to upstream `sf-*` skills. |

`npx skills add` does **not** register agents. For Claude Code, copy into your agents directory (see your tooling's docs), for example:

```bash
cp agents/sf-maps-architect.md ~/.claude/agents/
```

---

## Installation

Requires [Node.js 18+](https://nodejs.org/) for `npx`.

### Recommended (layered)

Install the base Salesforce skill library **first**, then this pack:

```bash
npx skills add forcedotcom/afv-library --yes
npx skills add erikboderek/sf-maps-skills --yes
```

Install a single skill from this repo:

```bash
npx skills add erikboderek/sf-maps-skills --skill sf-maps-territory
```

List skills before installing:

```bash
npx skills add erikboderek/sf-maps-skills --list
```

### Without `npx` (manual)

Copy each folder under `skills/` into your agent's skill directory (for example `~/.claude/skills/`), preserving the folder name so it matches the `name` field in each `SKILL.md`.

---

## Cursor, VS Code, and Agentforce Vibes

### Cursor

Clone or open a project that contains this **`skills/`** tree. In [Cursor](https://cursor.com), open that workspace so **`.cursor/skills/*/SKILL.md`** and **`.cursor/agents/`** are on disk; Cursor uses them as **project-local** agent guidance—tune AI / rules in Cursor **Settings** as needed.

### Visual Studio Code

In [Visual Studio Code](https://code.visualstudio.com/), open the repo and browse **`skills/*/SKILL.md`** as **documentation** while you work. For **skill-based AI** inside VS Code, install and use **Agentforce Vibes** (next section) or use **Cursor** with the same files under **`.cursor/`**.

### Agentforce Vibes

The Salesforce guide [Skills in Agentforce Vibes](https://developer.salesforce.com/docs/platform/einstein-for-devs/guide/skills.html) describes the **`SKILL.md`** format, progressive loading (`references/`, `assets/`, `scripts/`), and workspace layout:

| What | Where |
|------|--------|
| Project skills (recommended) | **`.a4drules/skills/`** at the root of your Salesforce / DX workspace |
| Enable the feature | VS Code **Settings** → **Agentforce Vibes** → **Enable Skills** (on by default) |

**To use this pack with Agentforce Vibes:** copy each folder under **`skills/`** (for example `sf-maps-setup`, `sf-maps-territory`, `sf-maps-field`) into **`<your-project>/.a4drules/skills/`**. Folder names must match the **`name`** in each `SKILL.md` frontmatter. Then open the **Skills** panel in Agentforce Vibes to verify they load.

`agents/sf-maps-architect.md` is for **Cursor / Claude-style** agents, not the Vibes skill loader—reuse the prose manually in Vibes if you want the same persona.

---

## Repository layout

```
.
├── README.md
├── agents/
│   └── sf-maps-architect.md
├── docs/
│   ├── adr-maps.md
│   ├── guide-agent-skills.md
│   └── sf-maps-legacy-patterns.md
└── skills/
    ├── sf-maps-setup/
    │   ├── SKILL.md
    │   └── references/
    │       ├── sf-maps-objects-setup.md
    │       └── sf-maps-objects-maps-namespace.md
    ├── sf-maps-territory/
    │   ├── SKILL.md
    │   └── references/
    │       └── sf-maps-objects-territory.md
    ├── sf-maps-field/
    │   ├── SKILL.md
    │   └── references/
    │       ├── sf-maps-data-layer-tooltips.md
    │       ├── sf-maps-shape-layer-countries.md
    │       └── sf-maps-reference-data.md
    ├── sf-maps-custom-actions/
    │   └── SKILL.md
    └── sf-maps-arcgis/
        └── SKILL.md
```

---

## Contributing

Open issues or PRs against this repository for skill text, references, or agent prompt updates. Keep guidance aligned with **native Salesforce Maps** capabilities unless an explicit exception is documented.
