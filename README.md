# Salesforce Maps — Agent Skills

A specialized skill library for **Salesforce Maps** (formerly MapAnything), providing territory management, field sales optimization, setup guidance, and developer API patterns for AI agents working in Salesforce orgs with Maps installed.

## Key Components

**Available Skills:**

- `sf-maps-setup`: Installation prerequisites, permission sets, org configuration, and geocoding setup
- `sf-maps-territory`: Territory management, ATM (Advanced Territory Management), coverage rules, and assignment logic
- `sf-maps-field`: Field sales workflows — routing, markers, data layers, visit planning, and mobile usage

**Agent:**

- `sf-maps-architect`: A technical architect persona integrating all Maps skills for admins and developers

## Installation Options

Install via `npx skills add` (requires Node.js 18+):

```bash
# Step 1 — Install the Salesforce base skills (required)
npx skills add forcedotcom/afv-library --yes

# Step 2 — Install Salesforce Maps skills
npx skills add erikboderek/sf-maps-skills --yes
```

Or manually copy skill folders to `~/.claude/skills/`.

## Integration Approaches

| Tool              | Location                    |
|-------------------|-----------------------------|
| Cursor            | `.cursor/skills/`           |
| VS Code           | `~/.claude/skills/`         |
| Agentforce Vibes  | `.a4drules/skills/`         |

All skills use the `sf-maps-*` prefix. The repository is published under the MIT license.
