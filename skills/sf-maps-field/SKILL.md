# sf-maps-field Skill

This skill guides **Salesforce Maps field sales workflows**: route optimization, markers, data layers, visit planning, check-ins, and mobile usage for field reps and their managers.

## When to Use This Skill

Trigger when the user is:
- Building or optimizing routes for field reps
- Configuring data layers (which Salesforce records appear on the map)
- Setting up visit scheduling, check-ins, or activity logging from Maps
- Asking about Salesforce Maps mobile app capabilities
- Designing a field sales workflow that involves the map view
- Configuring markers, custom marker icons, or color rules for map records

Do NOT trigger for:
- Initial Maps installation or permission setup (use `sf-maps-setup`)
- Territory design or ATM assignment rules (use `sf-maps-territory`)
- Generic Apex or Flow work unrelated to Maps field workflows (use upstream `sf-*` skills)

---

## Core Field Sales Objects

| Object | API Name | Purpose |
|---|---|---|
| Route | `SFMaps__Route__c` | A saved route for a rep on a given day |
| Route Stop | `SFMaps__RouteStop__c` | An individual stop on a route (linked to a Salesforce record) |
| Check-In | `SFMaps__CheckIn__c` | GPS-verified visit log when rep arrives at a stop |
| Marker | `SFMaps__Marker__c` | A custom point-of-interest pinned on the map (not tied to a Salesforce record) |
| Data Layer | `SFMaps__DataLayer__c` | Configuration for which object/record type appears as a map layer |
| Layer Filter | `SFMaps__LayerFilter__c` | Filter criteria applied to a Data Layer |

---

## Route Optimization

Routes are optimized for **daily field visits**. Key constraints:

| Parameter | Limit |
|---|---|
| Stops per route | Up to 25 (practical limit for one-day optimization) |
| Optimization algorithm | Nearest neighbor with time window support |
| Start/End location | Configurable per rep (home, office, or custom address) |
| Traffic | Real-time traffic factored into ETA calculations |

### Route Stop Statuses

| Status | Meaning |
|---|---|
| `Planned` | On the route, not yet visited |
| `Completed` | Rep checked in or marked complete |
| `Skipped` | Rep bypassed this stop |
| `Cancelled` | Removed from route |

---

## Data Layers

Data Layers control what records appear on the map and how they're styled. Each layer maps to one Salesforce object.

### Layer Configuration

| Setting | Notes |
|---|---|
| Object | Which SObject to display (Account, Lead, Opportunity, etc.) |
| Label Field | What text appears in the map pin tooltip |
| Lat/Lng Source | Must be a geocoded object (via Maps geocoding) |
| Filter | SOQL WHERE conditions to limit which records display |
| Color Rule | Field-based color coding (e.g., color accounts by `Industry` or `AnnualRevenue` range) |
| Icon | Custom icon per record type or value |
| Max Records | Default 5,000 records per layer (configurable in Maps Setup) |

### Common Layer Setups

**Accounts by ownership:**
- Object: `Account`
- Color Rule: `OwnerId = CURRENT_USER` → green; else → grey
- Filter: `SFMaps__GeocodingStatus__c = 'Geocoded'`

**Open opportunities by stage:**
- Object: `Opportunity`
- Color Rule: `StageName` → each stage a different color
- Filter: `IsClosed = false AND SFMaps__GeocodingStatus__c = 'Geocoded'`

---

## Check-Ins

When a rep arrives at a route stop, Maps can log a check-in:

- Creates a `SFMaps__CheckIn__c` record with GPS coordinates and timestamp
- Optionally creates a Salesforce Activity (Task or Event) on the parent record
- Configurable radius for "arrival detection" (default: 500 feet)

**Check-in → Activity logging:** Configure in Maps Setup → Mobile → Activity Logging. Map the check-in action to an Activity subject, type, and status.

---

## Visit Planning (Proximity Search)

Reps can use the **Nearby** panel to find records within a radius of their current location or a selected address:

- Searches geocoded records in any configured Data Layer
- Results sorted by distance
- Can add results directly to a route

---

## Mobile App

The Salesforce Maps mobile app (iOS and Android) supports:

- Full route view and navigation (turn-by-turn via native map app)
- Check-in at stops
- Nearby search
- Offline mode: routes and stop data cached locally for areas with poor connectivity
- Re-optimization: reps can re-optimize remaining stops on the fly

**Mobile configuration:** Maps Setup → Mobile → configure offline cache size, navigation app (Apple Maps, Google Maps, Waze), and check-in radius.

---

## SOQL Patterns

Find today's routes for a user:
```soql
SELECT Id, Name, SFMaps__Date__c, SFMaps__Status__c,
       (SELECT Id, SFMaps__Stop__c, SFMaps__Status__c FROM SFMaps__RouteStops__r ORDER BY SFMaps__StopOrder__c)
FROM SFMaps__Route__c
WHERE SFMaps__User__c = :userId
AND SFMaps__Date__c = TODAY
```

Find all check-ins this week:
```soql
SELECT Id, SFMaps__User__c, SFMaps__CheckInTime__c, SFMaps__Latitude__c, SFMaps__Longitude__c,
       SFMaps__Record__c
FROM SFMaps__CheckIn__c
WHERE SFMaps__CheckInTime__c = THIS_WEEK
ORDER BY SFMaps__CheckInTime__c DESC
```

Find skipped stops this month (rep performance):
```soql
SELECT SFMaps__Route__r.SFMaps__User__c, COUNT(Id) skipped
FROM SFMaps__RouteStop__c
WHERE SFMaps__Status__c = 'Skipped'
AND SFMaps__Route__r.SFMaps__Date__c = THIS_MONTH
GROUP BY SFMaps__Route__r.SFMaps__User__c
ORDER BY skipped DESC
```

---

**License:** MIT | **Version:** 1.0.0
