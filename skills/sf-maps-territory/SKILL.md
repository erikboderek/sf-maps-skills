# sf-maps-territory Skill

This skill guides **Salesforce Maps territory design, Advanced Territory Management (ATM), coverage rules, reference data layers, and territory assignment logic**.

## When to Use This Skill

Trigger when the user is:
- Designing or restructuring a territory hierarchy
- Configuring ATM assignment rules (geographic boundary, attribute-based, or manual)
- Setting up Reference Data layers (zip codes, counties, states, census tracts)
- Troubleshooting territory assignments or coverage gaps
- Planning rollup reporting across territory levels
- Migrating from Salesforce Enterprise Territory Management (ETM) to Maps ATM

Do NOT trigger for:
- Initial Maps installation or permission setup (use `sf-maps-setup`)
- Day-to-day routing or visit planning (use `sf-maps-field`)
- Generic Apex or Flow work unrelated to Maps territory logic (use upstream `sf-*` skills)

---

## Territory Model Options

| Model | When to Use |
|---|---|
| **Basic Territories** | Single-level, no overlap, small team (<50 reps), manual assignment |
| **Advanced Territory Management (ATM)** | Multi-level hierarchy, rule-based assignment, overlapping territories, enterprise scale |

Always ask: Does the org need overlapping territories (e.g., overlay reps covering the same geography as primary reps)? If yes → ATM required.

---

## ATM Core Objects

| Object | API Name | Purpose |
|---|---|---|
| Territory | `SFMaps__Territory__c` | A geographic or attribute-defined sales territory |
| Territory Hierarchy | `SFMaps__TerritoryHierarchy__c` | Defines parent-child levels (e.g., Region > Area > District) |
| Territory Assignment | `SFMaps__TerritoryAssignment__c` | Links a territory to a user (rep) with a role |
| Territory Record Assignment | `SFMaps__TerritoryRecordAssignment__c` | Links a Salesforce record (Account, etc.) to a territory |
| Assignment Rule | `SFMaps__AssignmentRule__c` | Auto-assignment rule (geographic boundary or attribute match) |
| Coverage Unit | `SFMaps__CoverageUnit__c` | A reference data unit (zip code, county) that belongs to a territory |

---

## Territory Types

| Type | API Value | Description |
|---|---|---|
| Geographic | `Geographic` | Drawn on the map using boundary polygons |
| Attribute | `Attribute` | Assigned based on Salesforce field values (e.g., Industry = 'Healthcare') |
| Manual | `Manual` | Individual records hand-assigned to the territory |
| Overlay | `Overlay` | Covers same geography as another territory (e.g., specialist overlay) |

---

## Reference Data

Reference Data provides pre-built geographic boundaries. Install the **Salesforce Maps Reference Data** package separately from the main Maps package.

Available boundary layers:

| Layer | Description |
|---|---|
| US ZIP Codes | 5-digit USPS zip code boundaries |
| US Counties | County-level boundaries |
| US States | State boundaries |
| US Census Tracts | Census tract boundaries |
| Canadian Postal Codes | FSA-level Canadian postal code boundaries |
| Canadian Provinces | Province boundaries |
| International (varies) | Country-specific postal/admin boundaries |

**Key Rule:** Use Reference Data layers to draw geographic territory boundaries on the map. Do not try to draw freehand polygon territories without a reference boundary layer — they cannot be maintained or assigned consistently.

---

## Assignment Rule Logic

ATM Assignment Rules auto-assign records to territories when records match defined criteria. Rule types:

1. **Boundary Rule** — record's geocoded lat/lng falls inside the territory boundary polygon
2. **Attribute Rule** — record field value matches a condition (e.g., `Account.BillingState = 'CA'`)
3. **Combined** — boundary AND attribute conditions applied together

Rules are evaluated in priority order. First matching rule wins (unless `Assign All Matching` is enabled for overlay territories).

---

## Hierarchy Levels

Define territory hierarchy levels before creating territories:

```
National
  └── Region (e.g., West, East, Central)
        └── Area (e.g., Northern CA, Southern CA)
              └── District (individual rep territory)
```

Best practice: Do not exceed 5 hierarchy levels. More than that indicates the territory model needs simplification, not deeper nesting.

---

## Territory Assignment Roles

Each `SFMaps__TerritoryAssignment__c` has a role:

| Role | Meaning |
|---|---|
| `Primary` | The rep who owns accounts in this territory |
| `Secondary` | Backup or overlay rep |
| `Manager` | Territory manager (read-only on territory records) |
| Custom roles | Configurable in Maps Setup → Territory Roles |

---

## SOQL Patterns

Find all territories for a given user:
```soql
SELECT SFMaps__Territory__r.Name, SFMaps__Role__c
FROM SFMaps__TerritoryAssignment__c
WHERE SFMaps__User__c = :userId
ORDER BY SFMaps__Territory__r.Name
```

Find accounts not assigned to any territory:
```soql
SELECT Id, Name, BillingState, SFMaps__GeocodingStatus__c
FROM Account
WHERE Id NOT IN (
  SELECT SFMaps__Record__c FROM SFMaps__TerritoryRecordAssignment__c
  WHERE SFMaps__Record__r.Type = 'Account'
)
AND SFMaps__GeocodingStatus__c = 'Geocoded'
```

---

**License:** MIT | **Version:** 1.0.0
