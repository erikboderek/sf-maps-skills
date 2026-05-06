# Architecture Decision Record — Salesforce Maps

## ADR-001: Permission Model

**Decision:** Assign permission sets — not profiles — for Maps access.

**Rationale:** Salesforce Maps ships three managed permission sets (`Maps_User`, `Maps_Advanced_User`, `Maps_Administrator`). Profile-based permissions conflict with the managed package's upgrade path. Always assign via permission sets so Maps updates don't break access.

**How to apply:** When a user reports access errors, check permission set assignment before touching profiles or field-level security.

---

## ADR-002: Geocoding Source

**Decision:** Use Salesforce Maps' built-in geocoding (MapAnything geocoder) rather than external services or formula-field coordinates.

**Rationale:** The geocoder runs asynchronously via the `SFMaps__Geocodable__c` and `SFMaps__GeocodingStatus__c` fields on any geocoded object. Custom geocoding solutions break routing and territory intersection calculations.

**How to apply:** When configuring which Salesforce objects appear on the map, ensure the Maps geocoding is enabled on the object via Maps Setup — not a custom lat/lng field approach.

---

## ADR-003: Territory Model Selection

**Decision:** Use Advanced Territory Management (ATM) for complex multi-level hierarchies; use Basic Territories for simple single-level rep assignment.

**Rationale:** ATM (`SFMaps__Territory__c` hierarchy with `SFMaps__TerritoryAssignment__c` rules) supports overlapping territories, rule-based assignment, and multi-level rollups. Basic territories are faster to set up but cannot support enterprise-scale routing or multi-rep coverage models.

**How to apply:** Ask about territory depth and overlap requirements before recommending. If reps share accounts across regions or need hierarchical rollup reporting, recommend ATM.

---

## ADR-004: Data Layer vs. Reference Data

**Decision:** Use Reference Data layers (census boundaries, zip codes, counties) for territory drawing; use Data Layers for live Salesforce record overlays.

**Rationale:** Reference Data is pre-loaded geographic boundary data (provided via the Salesforce Maps Reference Data package). Data Layers pull live records from Salesforce objects and display them as map markers or heatmaps. Mixing up these two causes confusion in territory design workflows.

**How to apply:** When a user wants to draw territory boundaries, point to Reference Data and the ATM boundary editor. When they want to see accounts/leads/opportunities on the map, point to Data Layers.

---

## ADR-005: Route Optimization Scope

**Decision:** Salesforce Maps routing is session-based and not a replacement for a full logistics platform.

**Rationale:** Maps optimizes daily visit routes for field reps (up to ~25 stops per route via the managed `SFMaps__Route__c` and `SFMaps__RouteStop__c` objects). It is not designed for fleet logistics, real-time tracking, or multi-day depot-based routing. Escalate those requirements to MuleSoft + a dedicated logistics provider.

**How to apply:** When scoping routing requirements, ask about stop count per day, multi-day horizons, and whether real-time vehicle tracking is needed. Cap route complexity expectations at Maps' supported model.
