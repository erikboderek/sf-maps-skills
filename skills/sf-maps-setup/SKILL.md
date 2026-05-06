# sf-maps-setup Skill

This skill guides **Salesforce Maps installation, org prerequisites, permission configuration, and geocoding setup**.

## When to Use This Skill

Trigger when the user is:
- Installing or upgrading the Salesforce Maps managed package
- Configuring permission sets for Maps users or admins
- Enabling or troubleshooting geocoding on Salesforce objects
- Setting up Maps for the first time in a sandbox or production org
- Asking about org prerequisites, license counts, or edition requirements

Do NOT trigger for:
- Territory design or ATM rule configuration (use `sf-maps-territory`)
- Field sales workflows, routing, or daily visit planning (use `sf-maps-field`)
- Generic Apex, Flow, or LWC work unrelated to Maps setup (use upstream `sf-*` skills)

---

## Org Prerequisites

| Requirement | Detail |
|---|---|
| Edition | Professional, Enterprise, Unlimited, or Developer |
| License type | Salesforce Maps Add-On license (one per user who needs access) |
| Maps Advanced | Separate add-on for Advanced Territory Management (ATM) |
| My Domain | Must be enabled and deployed before installing |
| Chatter | Must be enabled (Maps uses Chatter Feed for collaboration features) |
| Geocoding | Built-in Maps geocoder — do not use an external service |

---

## Package Installation

Salesforce Maps is a **managed package** installed from AppExchange. The package namespace is `SFMaps`. Post-installation steps:

1. Assign the `Maps_Administrator` permission set to the installing admin
2. Complete the Maps Setup wizard in Setup → Maps Setup
3. Enable geocoding on each object you want to display on the map
4. Assign `Maps_User` or `Maps_Advanced_User` permission sets to end users

---

## Permission Sets

| Permission Set | API Name | Use |
|---|---|---|
| Maps User | `Maps_User` | Standard field rep — can view map, create routes, use layers |
| Maps Advanced User | `Maps_Advanced_User` | Access to Advanced Territory Management features |
| Maps Administrator | `Maps_Administrator` | Full admin: setup, data layers, territory config, reference data |

**Never assign Maps access via profiles.** Always use these managed permission sets to stay compatible with package upgrades.

---

## Geocoding

Maps geocodes addresses asynchronously. Key fields added to geocoded objects:

| Field | API Name | Notes |
|---|---|---|
| Geocoding Status | `SFMaps__GeocodingStatus__c` | Values: `Geocoded`, `Failed`, `Pending`, `Not Geocoded` |
| Latitude | `SFMaps__Latitude__c` | Set after successful geocoding |
| Longitude | `SFMaps__Longitude__c` | Set after successful geocoding |
| Geocodable | `SFMaps__Geocodable__c` | Checkbox — must be true for the record to plot on the map |

**Enabling geocoding on an object:** Maps Setup → Geocoding → select the object → map the address fields → save. Maps queues existing records for batch geocoding.

**Common failure causes:**
- Address fields not mapped correctly in Maps Setup
- Record address is incomplete or malformed
- Daily geocoding quota exceeded (check Maps Setup → Geocoding Dashboard)

---

## Maps Setup Wizard Sections

1. **Users & Licenses** — assign permission sets, verify license counts
2. **Geocoding** — enable objects, map address fields, run batch geocode
3. **Data Layers** — configure which Salesforce objects display as map layers
4. **Reference Data** — install and configure geographic boundary datasets
5. **Territory Management** — enable ATM if licensed
6. **Mobile** — configure Salesforce Maps mobile app settings

---

**License:** MIT | **Version:** 1.0.0
