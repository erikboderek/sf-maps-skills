# Installing Salesforce Maps Agent Skills

## Prerequisites

- Node.js 18 or higher
- Claude Code CLI (`npm install -g @anthropic-ai/claude-code`) or Cursor

## Two-Step Installation

### Step 1 — Base Salesforce Skills (Required)

The Salesforce Maps skills delegate platform-level tasks (Apex, Flow, LWC, SOQL, OmniStudio, deployments) to the upstream library:

```bash
npx skills add forcedotcom/afv-library --yes
```

### Step 2 — Salesforce Maps Skills

```bash
npx skills add erikboderek/sf-maps-skills --yes
```

## Manual Installation

Copy the skill folders into your agent's skill directory:

```bash
cp -r skills/sf-maps-setup ~/.claude/skills/
cp -r skills/sf-maps-territory ~/.claude/skills/
cp -r skills/sf-maps-field ~/.claude/skills/
```

For the agent definition, copy separately:

```bash
cp agents/sf-maps-architect.md ~/.claude/agents/
```

## What's Included

| Component                | Purpose                                                   |
|--------------------------|-----------------------------------------------------------|
| `sf-maps-setup`          | Installation, permissions, geocoding, org config          |
| `sf-maps-territory`      | Territory design, ATM, coverage rules, assignment logic   |
| `sf-maps-field`          | Routing, markers, layers, visit planning, mobile usage    |
| `sf-maps-architect`      | Integrated architect agent for Maps projects              |

## Skill Trigger Philosophy

Each skill defines explicit **when to trigger** and **when not to trigger** rules. Delegate Apex, Flow, and LWC implementation to upstream `sf-*` skills from `forcedotcom/afv-library`. Use these Maps skills for domain-specific object selection, permission setup, and Maps-feature architecture.
