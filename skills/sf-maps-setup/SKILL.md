---
name: sf-maps-setup
version: 1.1.0
license: MIT
description: >
  ACTIVATE for Salesforce Maps package installation, org prerequisites, permission set
  assignment, permission group configuration, geocoding setup and batch jobs, Enhanced UX
  setup, Nearby Maps LWC, Auto Assignment, Mileage Administration, and Experience Cloud
  embedding. TRIGGER on: installing or upgrading Maps, configuring permission sets or groups,
  troubleshooting geocoding, enabling Enhanced UX remote sites, or setting up Maps for the
  first time in sandbox or production. DO NOT TRIGGER for territory design (sf-maps-territory),
  field rep routing or check-in workflows (sf-maps-field), or ArcGIS connectors (sf-maps-arcgis).
---

# sf-maps-setup Skill

This skill guides **Salesforce Maps installation, org prerequisites, permission configuration, and geocoding setup**.

## When to Use This Skill

Trigger when the user is:
- Installing or upgrading the Salesforce Maps managed package
- Configuring permission sets or permission groups for Maps users or admins
- Enabling or troubleshooting geocoding on Salesforce objects
- Setting up Maps for the first time in a sandbox or production org
- Asking about org prerequisites, license counts, or edition requirements
- Configuring Enhanced User Experience, Nearby Maps, Auto Assignment, or Mileage Administration
- Setting up Experience Cloud (partner site) embedding of Salesforce Maps

Do NOT trigger for:
- Territory design or ATM rule configuration (use `sf-maps-territory`)
- Field sales workflows, routing, or daily visit planning (use `sf-maps-field`)
- Generic Apex, Flow, or LWC work unrelated to Maps setup (use upstream `sf-*` skills)

---

## Hard-Stop Rules

If any of the following would be violated, stop and explain before proceeding:

| Constraint | Rationale |
|---|---|
| Never grant Maps access via profiles | Package upgrades reset profile-based field permissions; managed permission sets survive upgrades |
| Never use an external geocoding service on geocoded objects | Maps includes its own geocoder; external services create duplicate or conflicting coordinates on the same record |
| Always enable and deploy My Domain before installing the package | Package installation fails without an active My Domain |
| Always verify `SFMaps__GeocodingStatus__c = 'Geocoded'` before diagnosing map visibility | Ungeocodded records are the most common cause of "record not showing on map" — rule out geocoding before investigating layers |
| Never create permission groups without assigning a permission set first | A user in a permission group without the `SF Maps` (or equivalent) permission set cannot open Maps |

---

## Org Prerequisites

| Requirement | Detail |
|---|---|
| Edition | Professional, Enterprise, Performance, Unlimited, or Developer |
| License type | Salesforce Maps Add-On license (one per user who needs access) |
| Maps Advanced | Separate add-on for Advanced Territory Management (ATM) and automated routing |
| My Domain | Must be enabled and deployed before installing |
| Chatter | Must be enabled (Maps uses Chatter Feed for collaboration features) |
| Geocoding | Built-in Maps geocoder — do not use an external service |

---

## Package Installation

Salesforce Maps is a **managed package** installed from AppExchange. The package namespace is `SFMaps`. Post-installation steps:

1. Assign the `Maps Administrator` permission set to the installing admin
2. Complete the Maps Setup wizard in **Installed Packages → Configure**
3. Enable geocoding on each object you want to display on the map
4. Assign permission sets and add users to a **Permission Group**

---

## Permission Sets (Salesforce Platform)

These are managed Salesforce permission sets assigned in Setup → Permission Sets:

| Permission Set | API Name | Use |
|---|---|---|
| SF Maps | `SF_Maps` | Standard field rep — map access, routes, layers |
| SF Maps Advanced | `SF_Maps_Advanced` | Access to Advanced Routing (Maps Advanced) features |
| SF Maps Territory Planning | `SF_Maps_Territory_Planning` | Territory Planning features |
| SF Maps Territory Planning for Sales Planning | `SF_Maps_Territory_Planning_for_Sales_Planning` | Integration with Sales Planning |
| SF Maps Community Logins | `SF_Maps_Community_Logins` | Experience Cloud guest/login licenses |
| SF Maps Community Named User | `SF_Maps_Community_Named_User` | Named Experience Cloud users |
| SF Maps Live Admin | `SF_Maps_Live_Admin` | Live Tracking administration |
| SF Maps Live Mobile Tracking | `SF_Maps_Live_Mobile_Tracking` | Live Tracking mobile access |
| Maps Administrator | `Maps_Administrator` | Full admin: all setup, data layers, territory config |

**Hyperforce-specific permission sets:**
- `SF Maps Integration User`
- `SF Maps Platform Integration User`

**Never assign Maps access via profiles.** Always use these managed permission sets to stay compatible with package upgrades.

For Maps Advanced, reps need **both** `SF Maps` and `SF Maps Advanced`. Visit plan creators (managers) need `Maps Admin`, `SF Maps`, and `SF Maps Advanced`.

---

## Permission Groups (In-App Config)

Permission Groups are configured in **Installed Packages → Configure → Permission Groups** — not in Salesforce Setup. They control which Maps features and layers each user group can access.

Key Permission Group settings:

| Setting | What It Does |
|---|---|
| Folder Admin | Lets users manage folders in the shared Corporate folder |
| Show User Folders | Lets users see personal folders of other users (by role hierarchy) |
| Allow Marker Exports | Lets users export marker layer data to CSV |
| Show Weather Tab | Lets users view weather conditions for a marker's location |
| Show Personal Folder | Lets users access their personal folders |
| Enable ArcGIS Layers | Lets users manage ArcGIS layers |
| Manage Data Layers | Lets users create and view third-party data layers |
| Auto Check Out | Saves reps a step — checks out automatically on check-in, logs activity as complete |
| Enable Live Mobile Tracking | Gives live location tracking through mobile devices |
| Maximum Records for External Objects | Limits external object marker layers to 2,000 records |
| Button Set Name | Assigns a named button set to users in this group |
| Manage Territory Layers | Lets users create, modify, and aggregate territory layers |
| Plot on Load Layers | Layers that automatically plot when a rep opens Maps |
| Off-Platform Processing Location | Set to match your Salesforce instance region for data residency (required for Maps Advanced) |

Every user must be in at least one permission group to use Maps. Permission groups also control **Timesheet / Mileage Administration** settings for distance reimbursement.

---

## Button Sets

Button Sets control which action buttons appear in the Maps UI for different contexts. Configure in **Installed Packages → Configure → Button Sets**, then assign via Permission Group → Button Set Name.

| Layout | What It Does |
|---|---|
| Popup | Action buttons on the Actions tab when reps select a specific marker |
| Mass Action | Action buttons when users select multiple markers for a mass action |
| My Position | Action buttons when users select their own location |
| POI | Action buttons when users click Point of Interest search results |

---

## Enhanced User Experience

The Enhanced User Experience (Enhanced Domains / Enhanced UX) is an opt-in mode that enables high-volume ArcGIS layers, modern list views, and improved map performance. Enable in **Installed Packages → Configure → Advanced → Use enhanced domains for nearby maps**.

**Prerequisites — three Remote Site Settings required:**

```
https://osmtiles.hereapi.com
https://vectortiles.hereapi.com
https://geocoder.hereapi.com
```

**Enhanced UX limitations (as of Feb 2026):**

- Territory layers are not supported
- Polyline layers are not supported in Enhanced UX mode
- Requires WebGL2 (modern browsers only)

---

## Geocoding

Maps geocodes addresses asynchronously. Key fields added to geocoded objects:

| Field | API Name | Notes |
|---|---|---|
| Geocoding Status | `SFMaps__GeocodingStatus__c` | Values: `Geocoded`, `Failed`, `Pending`, `Not Geocoded` |
| Latitude | `SFMaps__Latitude__c` | Set after successful geocoding |
| Longitude | `SFMaps__Longitude__c` | Set after successful geocoding |
| Geocodable | `SFMaps__Geocodable__c` | Checkbox — must be true for the record to plot on the map |

**Enabling geocoding on an object:** Installed Packages → Configure → Base Objects → select the object → map address fields → save.

**Common failure causes:**
- Address fields not mapped correctly in Base Objects config
- Record address is incomplete or malformed
- Daily geocoding quota exceeded

### Geocoding Batch Jobs

Schedule or run batch geocoding from **Installed Packages → Configure → Automation**:

| Tab | Purpose |
|---|---|
| Schedule New Batch | Schedule a recurring geocoding batch (weekly or monthly) |
| Run Manual Batch | One-off geocoding batch (processes up to 250,000 records in increments of 10) |
| Last 5 Batches Ran | Results of the five most recent batch runs |
| Scheduled Batches | View upcoming scheduled geocoding jobs |

**Batch options:**
- **Start Geocoding Batch** — geocode all un-geocoded records for a base object
- **Start Lat/Long Removal Batch** — clear existing coordinates (max 500 records) to force a fresh geocode
- **Start Clear Skip GeoCode Flag Batch** — reset the skip flag on records with malformed addresses (requires creating a `MASkipGeocoding__c` Checkbox field and enabling Skip Failed Geocodes in Base Object settings)

Required permissions: `Customize Application` + `API Enabled` + access to `ApexClass.Name`.

---

## Nearby Maps (LWC)

The **Maps Nearby Map** Lightning Web Component embeds a nearby-record map on any Lightning record page or Experience Cloud site.

### Add to a Record Page

1. Open the record page in Lightning App Builder
2. Drag **Maps Nearby Map** to the layout
3. Configure component properties: Height, Width, Nearby Map (select a saved global nearby map)

**Requirement:** Clickjack protection must be disabled in Setup → Session Settings.

### Add to an Experience Cloud Site

1. Installed Packages → Configure → Advanced → enable **Use enhanced domains for nearby maps**
2. Setup → All Sites → select site → Builder
3. Drag **Maps Nearby Map** from Custom Components to the layout
4. Set **Community Path** to your site domain, **Nearby Map** to the desired layer
5. Publish the site

### Create Custom Nearby Maps

In Installed Packages → Configure → Nearby Map → Create New:

| Mode | Description |
|---|---|
| This Record and Nearby | Shows the current record plus nearby records from a layer |
| Global (filtered) | Shows a predefined, filtered layer regardless of which record is open |

---

## Auto Assignment

Auto Assignment automatically assigns Salesforce records to territory owners based on geographic shape layers. Configure in **Installed Packages → Configure → Auto Assignment**.

### Setup Flow

1. **Configure Record Assignments** — select which object to assign (e.g., Account) and which user field to update (e.g., `OwnerId`)
2. **Set Up Assignment Plans** — create named plans (up to 50) with a shape layer, priority order, and a SOQL-style filter to scope which records are eligible
3. **Determine Assignment Rules** — within each plan, create up to 5,000 rules mapping specific shape layer polygons to specific users
4. **Schedule Assignment Runs** — auto-assign on a schedule or run manually

**How it works:** When a run executes, each eligible record's geocoded lat/lng is tested against the shape layer polygons in priority order. The first matching rule's assigned user is written to the configured field.

---

## Mileage Administration

Configure distance reimbursement and timesheets in **Installed Packages → Configure → Mileage Administration**.

| Tab | Purpose |
|---|---|
| Periods | Define timesheet periods (Weekly, Biweekly, Semi-Monthly) with a start date |
| Reimbursement Rules | Create rules: Starting Distance (minimum miles before reimbursement) or Stop-Based |
| Schedule Timesheet Creation | Schedule the batch job that creates timesheet records for reps |

**Key considerations:**
- Rules and periods are assigned via Permission Groups
- Removing a timesheet period from a permission group hides existing timesheets (restoring it makes them reappear)
- Timesheet activities require the rep's user ID in the `AssignedTo` field and status = `Completed`
- Set **Activity Generation Method** in Routes & Schedule: `Check In` (captures check-in timestamps) or `Auto Generated` (uses scheduling resource and start/end fields)

---

## Plot Specific Layers on Session Start

Configure layers to automatically plot when reps open Maps. Applies to Marker Layers and ArcGIS layers. **Data layers cannot be plotted on load.**

**Per layer:** On the Recent or Saved tab, three-dot menu → **Toggle Plot on Load**.

**Per permission group:** Edit the permission group → Layers section → search **Plot on Load Layers** → select layers to auto-plot for the group.

---

## Embed Salesforce Maps in Experience Cloud (Partner Sites)

1. Setup → All Sites → select the site → Builder
2. Drag **Visualforce Page** component to the layout
3. Set **Visualforce Page Name** to `Maps`
4. Set height to at least 400 pixels
5. Preview and publish

---

## Maps Setup Wizard Sections (Installed Packages → Configure)

| Section | Purpose |
|---|---|
| Base Objects | Enable geocoding per object, map address fields, set Check-In Settings, Schedule Priority Field, Map It Button Settings |
| Automation | Schedule or run geocoding batch jobs |
| Click2Create™ | Configure POI-to-record creation (Leads from property data, Accounts from business data) |
| Routes & Schedule | Configure event types, custom events, excluded event types for scheduling |
| Data Layers | Configure third-party data layers (business data, property data) |
| Nearby Map | Create and manage custom nearby maps |
| ArcGIS Connectors | Configure batch push of Salesforce records to ArcGIS Online/Enterprise feature services — see `sf-maps-arcgis` |
| Auto Assignment | Configure record assignment plans and rules |
| Advanced | System settings: Enhanced Domains, routing geometry, show subordinates default |
| Territory Management | Enable ATM if licensed |
| OAuth | Designate and authorize OAuth user for Maps Advanced route optimization |
| Mileage Administration | Timesheet periods, reimbursement rules, schedule creation job |
| Maps Advanced → Data Management | Schedule Advanced Batches for visit plan data sync |
| Permission Groups | Create and manage in-app permission groups |

---

## Supported Editions

Salesforce Maps is available in: **Professional, Enterprise, Performance, Unlimited, Developer**

Available as an add-on for: Sales Cloud, Service Cloud, Consumer Goods Cloud

License limit: you can purchase as many Maps licenses as your company has eligible Salesforce user subscriptions.

For object-level field references, see `references/sf-maps-objects-maps-namespace.md`.

---

## Delegate To

| Need | Skill |
|---|---|
| Territory design, ATM hierarchy, or assignment rules | `sf-maps-territory` |
| Route optimization, check-ins, markers, or field rep workflows | `sf-maps-field` |
| Map It buttons, Popup Flows, or multi-record custom actions | `sf-maps-custom-actions` |
| ArcGIS Living Atlas layers or ArcGIS Connectors batch push | `sf-maps-arcgis` |
| Apex trigger or class on a Maps object | `generating-apex` (afv-library) |
| Flow triggered by a Maps event (check-in, route complete) | `generating-flow` (afv-library) |
| LWC component that reads or writes Maps objects | `generating-lwc` (afv-library) |
| Deploying Maps package config to production | `deploying-metadata` (afv-library) |
