---
name: sf-maps-arcgis
version: 1.0.0
license: MIT
description: >
  ACTIVATE for ArcGIS integration with Salesforce Maps in either direction: consuming
  ArcGIS Living Atlas or custom Esri feature service layers on the map, or publishing
  Salesforce records to ArcGIS Online or ArcGIS Enterprise via ArcGIS Connectors (Batch
  Push). TRIGGER on: adding an ArcGIS feature service layer to Maps, configuring ArcGIS
  Connectors, scheduling or troubleshooting a batch push run, or enabling ArcGIS layer
  access in a permission group. DO NOT TRIGGER for general Maps installation or permission
  setup (sf-maps-setup) or marker/data layers sourced from Salesforce records (sf-maps-field).
---

# sf-maps-arcgis Skill

This skill covers both directions of ArcGIS integration in Salesforce Maps: **consuming** ArcGIS Living Atlas and custom feature service layers on the map, and **publishing** Salesforce record data to ArcGIS Online or ArcGIS Enterprise via ArcGIS Connectors (Batch Push).

## When to Use This Skill

Trigger when the user is:
- Adding an ArcGIS Living Atlas or custom feature service layer to the Salesforce Maps UI
- Configuring ArcGIS Connectors to push Salesforce records to an ArcGIS Online or ArcGIS Enterprise feature service
- Scheduling, running, or troubleshooting an ArcGIS Batch Push
- Enabling or permissioning ArcGIS layer access for a user group

Do NOT trigger for:
- General Salesforce Maps installation, permission sets, or geocoding setup (use `sf-maps-setup`)
- Marker layers or data layers sourced from Salesforce records (use `sf-maps-field`)

---

## Hard-Stop Rules

If any of the following would be violated, stop and explain before proceeding:

| Constraint | Rationale |
|---|---|
| Always verify the Permission Group has Enable ArcGIS Layers before diagnosing missing ArcGIS layer options | The ArcGIS Layer type does not appear in the UI at all without this toggle — no amount of URL or auth troubleshooting will fix a missing permission |
| Always run a test push before scheduling a live batch push | Live batch pushes overwrite feature service data; a misconfigured field mapping corrupts the ArcGIS dataset with no automatic rollback |
| Do not confuse the ArcGIS Connectors OAuth with the Maps Advanced OAuth | The Connectors OAuth authenticates to Esri (ArcGIS Online/Enterprise); the Maps Advanced OAuth authenticates to Salesforce for route optimization — they are separate settings |
| Only push records with `SFMaps__GeocodingStatus__c = 'Geocoded'` to ArcGIS | Records without valid coordinates produce null geometry features in the ArcGIS feature service, which breaks spatial queries downstream |

---

## Two Directions

| Direction | What it does | Where configured |
|-----------|-------------|-----------------|
| **Consume** | Pull ArcGIS Living Atlas or custom Esri feature service data onto the map for visualization | Maps UI → Layers → New → ArcGIS Layer |
| **Publish** | Push geocoded Salesforce records to an ArcGIS Online / Enterprise feature service on a schedule | Installed Packages → Configure → ArcGIS Connectors |

Use **Consume** when reps need to see external GIS data (environmental boundaries, infrastructure, demographics) alongside Salesforce markers. Use **Publish** when ArcGIS Online/Enterprise dashboards or workflows need up-to-date Salesforce data.

---

## Consuming ArcGIS Layers

### Permission Requirement

The user's Maps Permission Group must have **Enable ArcGIS Layers** turned on. Without it, the ArcGIS Layer option does not appear in the layer creation UI.

### Adding a Layer

1. Open Salesforce Maps → **Layers** tab → **New**
2. Select **ArcGIS Layer**
3. Paste the Esri **Feature Service URL** (from ArcGIS Living Atlas or your organization's ArcGIS Online/Enterprise portal)
4. Name the layer and save

The layer is added to the user's personal folder. Share it to the Corporate folder to make it available to other reps.

### Enhanced User Experience

ArcGIS layers are fully supported in Enhanced UX mode and perform better there for high-volume datasets. Enable Enhanced UX in **Installed Packages → Configure → Advanced → Use enhanced domains for nearby maps**.

### Plot on Load

ArcGIS layers support **Plot on Load** (unlike Data Layers). Configure per layer (three-dot menu → Toggle Plot on Load) or per permission group (Permission Groups → Layers → Plot on Load Layers).

---

## Publishing: ArcGIS Connectors (Batch Push)

ArcGIS Connectors push geocoded Salesforce records to a target ArcGIS feature service on a configurable schedule. Each connector maps a Salesforce object to an ArcGIS feature layer and syncs records in bulk.

### Setup Steps

1. **Open ArcGIS Connectors:** Installed Packages → Configure → ArcGIS Connectors
2. **Authenticate:** Sign in to ArcGIS Online or ArcGIS Enterprise using OAuth. This is separate from the Maps Advanced OAuth user — it authenticates to Esri, not to Salesforce.
3. **Create a connector:** Select the Salesforce object to push (must be geocoded via Maps), enter the target ArcGIS feature service URL, and map Salesforce fields to ArcGIS feature service fields.
4. **Run a test push:** Use the **Test** action to validate field mappings and connectivity before going live. Results are written to `maps__ArcGISBatchPushTest__c`.
5. **Set a schedule:** Choose a recurring schedule or run manually.
6. **Go live:** Run the connector. Each run writes a result record to `maps__ArcGISBatchPushLog__c`.

### How It Works

When a batch push runs:
- Only records with `SFMaps__GeocodingStatus__c = 'Geocoded'` (or `maps__` equivalent) are eligible
- Salesforce field values are pushed to the ArcGIS feature service as feature attributes
- Geometry (lat/lng) is sent as the feature's point geometry
- Failures are logged on the `maps__ArcGISBatchPushLog__c` record — check `maps__ErrorCount__c` after each run

---

## Relevant Objects

| Object | API Name | Purpose |
|--------|----------|---------|
| ArcGIS Batch Push Setting | `maps__ArcGISBatchPushSetting__c` | One connector definition: target feature service URL, OAuth auth, field mappings, schedule |
| ArcGIS Batch Push Log | `maps__ArcGISBatchPushLog__c` | System-written run result; child of `maps__ArcGISBatchPushSetting__c`; stores record counts, errors, timestamp |
| ArcGIS Batch Push Test | `maps__ArcGISBatchPushTest__c` | Dry-run validation result; created by the Test action before a live push |
| Layer | `maps__Layer__c` | Layer record; `maps__ArcGISWebMapC2C__c` (TEXTAREA) stores ArcGIS integration config |

---

## SOQL Patterns

Recent batch push runs for a connector:

```soql
SELECT maps__ArcGISBatchPushSetting__r.Name, maps__RecordsProcessed__c,
       maps__ErrorCount__c, CreatedDate
FROM maps__ArcGISBatchPushLog__c
WHERE maps__ArcGISBatchPushSetting__c = :settingId
ORDER BY CreatedDate DESC
LIMIT 10
```

All connectors with recent failures:

```soql
SELECT Name, maps__FeatureServiceUrl__c, maps__LastRunDate__c
FROM maps__ArcGISBatchPushSetting__c
WHERE Id IN (
  SELECT maps__ArcGISBatchPushSetting__c
  FROM maps__ArcGISBatchPushLog__c
  WHERE maps__ErrorCount__c > 0
  AND CreatedDate = LAST_N_DAYS:7
)
```

---

## Delegate To

| Need | Skill |
|---|---|
| Maps package installation, permission sets, or permission group setup | `sf-maps-setup` |
| Marker layers, data layers, or field rep workflows | `sf-maps-field` |
| Apex class to process or react to ArcGIS batch push results | `generating-apex` (afv-library) |
| Flow triggered by a batch push completion | `generating-flow` (afv-library) |
| Deploying ArcGIS connector config to production | `deploying-metadata` (afv-library) |
