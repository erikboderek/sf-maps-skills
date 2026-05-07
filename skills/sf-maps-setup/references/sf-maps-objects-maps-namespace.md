# Salesforce Maps — Core `maps__` Namespace Objects

All objects use the `maps__` managed package namespace.

> **Note on dual namespaces:** Salesforce Maps uses two namespace prefixes. The `maps__` objects (Part 1 below) handle layer configuration, geocoding setup, custom actions, and folder organization — always present. Routing, check-ins, and data layer objects use the `SFMaps__` prefix (Part 2 below) and are installed as part of the routing module — verify their presence in your org. Territory management objects (`maps__TP*`) are in Part 1 at the end of this file.

---

## maps__BaseObject__c — Maps Base Object

One record per Salesforce object type that has geocoding enabled. This is the geocoding configuration registry. The `Name` field holds the SObject API name.

### Address field mapping

| Field | API Name | Type | Notes |
|---|---|---|---|
| Name | `Name` | STRING | SObject API name (e.g., `Account`) |
| Street | `maps__Street__c` | STRING | Field API name mapped to street |
| City | `maps__City__c` | STRING | Field API name mapped to city |
| State | `maps__State__c` | STRING | Field API name mapped to state |
| Postal Code | `maps__PostalCode__c` | STRING | Field API name mapped to zip |
| Country | `maps__Country__c` | STRING | Field API name mapped to country |
| Address Object | `maps__AddressObject__c` | STRING | For compound address fields |
| Polymorphic Address Object | `maps__PolymorphicAddressObject__c` | STRING | For polymorphic lookup address sources |

### Geocoding result fields

| Field | API Name | Type | Notes |
|---|---|---|---|
| Latitude | `maps__Latitude__c` | STRING | Geocoded latitude (stored as string) |
| Longitude | `maps__Longitude__c` | STRING | Geocoded longitude (stored as string) |
| Verified Latitude | `maps__VerifiedLatitude__c` | STRING | Manually verified override |
| Verified Longitude | `maps__VerifiedLongitude__c` | STRING | Manually verified override |
| Quality | `maps__Quality__c` | STRING | Geocoder confidence score |
| Similarity | `maps__Similarity__c` | STRING | Address match similarity score |
| Last Updated | `maps__LastUpdated__c` | DATETIME | Last geocode attempt timestamp |
| Processing | `maps__Processing__c` | BOOLEAN | True if batch geocoding in progress |
| Skip Failed Geocodes | `maps__SkipFailedGeocodes__c` | BOOLEAN | Skip records that previously failed |

### Configuration flags

| Field | API Name | Type | Notes |
|---|---|---|---|
| Type | `maps__Type__c` | PICKLIST | Object category |
| In Active | `maps__InActive__c` | BOOLEAN | Soft-delete flag for the config record |
| Settings | `maps__Settings__c` | TEXTAREA | JSON blob for additional geocoding config |
| Record Type Id | `maps__RecordTypeId__c` | STRING | Restrict geocoding to a specific record type |
| Batch Size | `maps__BatchSize__c` | DOUBLE | Records per geocoding batch job |
| Disable Global Search | `maps__DisableGlobalSearch__c` | BOOLEAN | Exclude from Maps global search |

### MapIt (proximity / Map It button) settings

| Field | API Name | Type | Notes |
|---|---|---|---|
| MapIt Proximity On | `maps__MapIt_Proximity_On__c` | BOOLEAN | Enable proximity search from map pin |
| MapIt Proximity Radius | `maps__MapIt_Proximity_Radius__c` | DOUBLE | Default search radius |
| MapIt Proximity Measurement Unit | `maps__MapIt_Proximity_Measurement_Unit__c` | PICKLIST | Miles or kilometers |
| MapIt Zoom Control | `maps__MapIt_Zoom_Control__c` | DOUBLE | Default zoom level for Map It |
| MapIt Zoom To Fit | `maps__MapIt_Zoom_To_Fit__c` | BOOLEAN | Auto-zoom to fit all pins |
| MapIt Show Inside Shape | `maps__MapIt_Show_Inside_Shape__c` | BOOLEAN | Highlight records inside selected shape |

### Routing settings

| Field | API Name | Type | Notes |
|---|---|---|---|
| Routing Start Time | `maps__RoutingStartTime__c` | STRING | Default route start time |
| Routing End Time | `maps__RoutingEndTime__c` | STRING | Default route end time |
| Routing Has Address | `maps__RoutingHasAddress__c` | STRING | Whether object has a routable address |
| Routing Is Flexible | `maps__RoutingIsFlexible__c` | STRING | Allow flexible time windows |

### Tooltip fields (1–15)

`maps__Tooltip1__c` through `maps__Tooltip15__c` — all STRING. Stores field API names to display in map pin tooltips. Also present identically on `maps__MarkerLayer__c`.

---

## maps__MarkerLayer__c — Maps Marker Layer

The primary layer configuration object. Defines what records appear on the map, how they are queried, styled, and what appears in tooltips. 58 fields total.

### Identity and ownership

| Field | API Name | Type | Notes |
|---|---|---|---|
| Name | `Name` | STRING | Layer display name |
| Folder | `maps__Folder__c` | REFERENCE → maps__Folder__c | Folder this layer belongs to |
| User | `maps__User__c` | REFERENCE → User | Owner |
| Organization Wide | `maps__OrgWide__c` | BOOLEAN | Visible to all org users |

### Data source

| Field | API Name | Type | Notes |
|---|---|---|---|
| Base Object | `maps__BaseObject__c` | REFERENCE → maps__BaseObject__c | Which SObject to query |
| Custom Query | `maps__CustomQuery__c` | BOOLEAN | True = use maps__Query__c directly; false = build from components |
| Query | `maps__Query__c` | TEXTAREA | SOQL SELECT statement (when Custom Query = true) |
| Query Key | `maps__QueryKey__c` | TEXTAREA | Cache key for query |
| Query Components | `maps__QueryComponents__c` | DOUBLE | Number of sub-queries |
| Activity Filter | `maps__ActivityFilter__c` | STRING | Filter for activity-linked records |
| Filter Logic | `maps__FilterLogic__c` | STRING | AND/OR logic for multi-condition filters |
| Picklist Field | `maps__PicklistField__c` | STRING | Field used for picklist-based color assignment |

### Display and styling

| Field | API Name | Type | Notes |
|---|---|---|---|
| Color Assignment | `maps__ColorAssignment__c` | TEXTAREA | JSON rules: field value → color mapping |
| Color Assignment Type | `maps__ColorAssignmentType__c` | PICKLIST | `field`, `range`, `dynamic` |
| Colors Assigned Dynamically | `maps__ColorsAssignedDynamically__c` | BOOLEAN | Auto-assign colors from a pool |
| Icon Color | `maps__IconColor__c` | STRING | Hex color for the layer icon |
| Shape Assignment | `maps__ShapeAssignment__c` | TEXTAREA | JSON: assign shapes to records by field value |
| Shape Field | `maps__ShapeField__c` | STRING | Field API name used for shape assignment |
| Advanced Options | `maps__AdvancedOptions__c` | TEXTAREA | JSON for non-standard display options |

### Tooltip fields (1–15)

`maps__Tooltip1__c` through `maps__Tooltip15__c` — all STRING. Each stores a field API name displayed in the map pin popup, in order.

### Performance and limits

| Field | API Name | Type | Notes |
|---|---|---|---|
| Max Query Size | `maps__MaxQuerySize__c` | DOUBLE | Max records from Salesforce data |
| Max Query Size External | `maps__MaxQuerySizeExternal__c` | DOUBLE | Max records from external data sources |
| Row Limit | `maps__RowLimit__c` | DOUBLE | Hard cap on displayed records |
| Row Order | `maps__RowOrder__c` | STRING | Field API name for ORDER BY |
| Row Order Direction | `maps__RowOrderDirection__c` | STRING | `ASC` or `DESC` |
| Related List Count | `maps__RelatedListCount__c` | DOUBLE | Number of related list queries |
| Refresh Interval | `maps__RefreshInterval__c` | STRING | Auto-refresh cadence |

### Proximity and sharing

| Field | API Name | Type | Notes |
|---|---|---|---|
| Proximity Options | `maps__ProximityOptions__c` | TEXTAREA | JSON for proximity search config |
| Owner Filter Id | `maps__OwnerFilterId__c` | STRING | Filter layer to a specific owner |
| Role Id | `maps__RoleId__c` | STRING | Restrict to users in a role hierarchy |
| Description | `maps__Description__c` | TEXTAREA | Admin notes |

---

## maps__Layer__c — Maps Layer

A container/wrapper object for layer grouping or aggregation. Less commonly queried directly than `maps__MarkerLayer__c`.

| Field | API Name | Type | Notes |
|---|---|---|---|
| Name | `Name` | STRING | Layer name |
| Type | `maps__Type__c` | PICKLIST | Layer category |
| Folder | `maps__Folder__c` | REFERENCE → maps__Folder__c | |
| User | `maps__User__c` | REFERENCE → User | |
| Description | `maps__Description__c` | TEXTAREA | |
| Options | `maps__Options__c` | TEXTAREA | JSON config blob |
| Version | `maps__Version__c` | DOUBLE | Schema version |
| ArcGIS WebMap C2C | `maps__ArcGISWebMapC2C__c` | TEXTAREA | ArcGIS integration config |
| Email On Aggregation Success | `maps__EmailOnAggregationSuccess__c` | EMAIL | |
| External ID | `External_ID__c` | STRING | Integration key |

---

## maps__ShapeLayer__c — Maps Shape Layer

Configuration record for a shape layer (geographic boundary overlay).

| Field | API Name | Type | Notes |
|---|---|---|---|
| Name | `Name` | STRING | Shape layer name |
| Folder | `maps__Folder__c` | REFERENCE → maps__Folder__c | |
| User | `maps__User__c` | REFERENCE → User | |
| Parent Shape Layer | `maps__ShapeLayer__c` | REFERENCE → maps__ShapeLayer__c | Self-lookup for nested shape layers |
| Custom Geometry | `maps__CustomGeometry__c` | BOOLEAN | True if drawn manually (lasso tool) |
| Job Id | `maps__JobId__c` | STRING | Background job ID for large boundary imports |
| Description | `maps__Description__c` | TEXTAREA | |
| Options | `maps__Options__c` | TEXTAREA | Display config JSON (merge boundaries, labels, etc.) |

---

## maps__ShapeLayerGeometry__c — Maps Shape Layer Geometry

Stores the GeoJSON polygon data for a shape layer. Large polygons are split across multiple TEXTAREA fields.

| Field | API Name | Type | Notes |
|---|---|---|---|
| Name | `Name` | STRING | Geometry record name |
| Shape Layer | `maps__ShapeLayer__c` | REFERENCE → maps__ShapeLayer__c | Parent layer |
| Geometry | `maps__Geometry__c` | TEXTAREA | Primary GeoJSON chunk |
| Geometry 2–10 | `maps__Geometry2__c` … `maps__Geometry10__c` | TEXTAREA | Overflow chunks for large polygons |
| Geometry Size | `maps__GeometrySize__c` | DOUBLE | Total geometry byte size |
| Description | `maps__Description__c` | STRING | Label for the shape (e.g., county or zip code name) |

When reading a shape's full geometry, concatenate `maps__Geometry__c` + `maps__Geometry2__c` … `maps__Geometry10__c` in order. Stop when a field is null.

---

## maps__Location__c — Maps Location

Saved location record — stores addresses and coordinates for use as route start/end points, favorites, and schedule stops. Unlike `maps__BaseObject__c`, latitude and longitude here are **DOUBLE** type.

| Field | API Name | Type | Notes |
|---|---|---|---|
| Name | `Name` | STRING | Location display name |
| Address | `maps__Address__c` | TEXTAREA | Full address string |
| Address Fields | `maps__AddressFields__c` | TEXTAREA | Structured address JSON |
| Latitude | `maps__Latitude__c` | DOUBLE | Decimal degrees |
| Longitude | `maps__Longitude__c` | DOUBLE | Decimal degrees |
| Date Time | `maps__DateTime__c` | DATETIME | Schedule timestamp |
| Is Start | `maps__IsStart__c` | BOOLEAN | Route start point |
| Is End | `maps__IsEnd__c` | BOOLEAN | Route end point |
| Is Schedule | `maps__IsSchedule__c` | BOOLEAN | Scheduled stop |
| Favorite Marker | `maps__FavoriteMarker__c` | TEXTAREA | JSON for saved/favorite marker config |
| Maps Folder | `maps__MapsFolder__c` | REFERENCE → maps__Folder__c | |
| Resource Id | `maps__ResourceId__c` | STRING | External resource identifier |
| User | `maps__User__c` | REFERENCE → User | |
| Description | `maps__Description__c` | TEXTAREA | |

---

## maps__Folder__c — Maps Folder

Hierarchical folder structure for organizing Marker Layers, Shape Layers, Layers, and Locations. Supports self-referential nesting.

| Field | API Name | Type | Notes |
|---|---|---|---|
| Name | `Name` | STRING | Folder display name |
| Parent Folder | `maps__ParentFolder__c` | REFERENCE → maps__Folder__c | Self-lookup for nested folders |
| User | `maps__User__c` | REFERENCE → User | Owner |
| Order | `maps__Order__c` | DOUBLE | Display sort order |
| Role Id | `maps__RoleId__c` | STRING | Restrict folder visibility to a role |
| Profile Ids | `maps__ProfileIds__c` | TEXTAREA | Comma-separated profile IDs with access |

---

## maps__FolderPermission__c — Maps Folder Permission

Per-user or per-profile CRUD permissions on a Maps folder. Each record grants a specific combination of permissions to one user or profile.

| Field | API Name | Type | Notes |
|---|---|---|---|
| Name | `Name` | STRING | Permission record name |
| Folder | `maps__Folder__c` | REFERENCE → maps__Folder__c | Target folder |
| User | `maps__User__c` | REFERENCE → User | Grantee (user-level) |
| Profile Id | `maps__ProfileId__c` | STRING | Grantee (profile-level) |
| Create | `maps__Create__c` | BOOLEAN | Can create items in folder |
| Read | `maps__Read__c` | BOOLEAN | Can view folder contents |
| Modify | `maps__Modify__c` | BOOLEAN | Can edit items |
| Delete | `maps__Delete__c` | BOOLEAN | Can delete items |
| Export | `maps__Export__c` | BOOLEAN | Can export layer data |
| Set Permissions | `maps__SetPermissions__c` | BOOLEAN | Can manage folder permissions |

---

## maps__CustomAction__c — Maps Custom Action

Defines a custom action button displayed in the Maps interface (tooltip, mass action, POI, or custom address layouts). Referenced by `maps__ButtonSet__c`.

| Field | API Name | Type | Notes |
|---|---|---|---|
| Name (Label) | `Name` | STRING | Button label shown in Maps |
| Type | `maps__Type__c` | PICKLIST | Action category |
| Action | `maps__Action__c` | PICKLIST | Behavior type (e.g., "Load page in new window") |
| Action Value | `maps__ActionValue__c` | TEXTAREA | URL, Flow API name, or `[Custom]` JS block |
| Base Objects | `maps__BaseObjects__c` | MULTIPICKLIST | Which SObjects this action applies to |
| Layouts | `maps__Layouts__c` | MULTIPICKLIST | Where the button appears (tooltip, mass action, POI, custom address) |
| Modes | `maps__Modes__c` | MULTIPICKLIST | Desktop, Mobile, or both |
| Requirements | `maps__Requirements__c` | MULTIPICKLIST | Conditions required before button is enabled |
| Options | `maps__Options__c` | TEXTAREA | JSON for additional config |

---

## maps__ButtonSet__c — Maps Button Set

Stores the JSON layout configuration for each of the five Maps interface panels. Each TEXTAREA field holds a JSON array of button references.

| Field | API Name | Type | Notes |
|---|---|---|---|
| Name (Label) | `Name` | STRING | Button set name |
| Tooltip Layout | `maps__TooltipLayout__c` | TEXTAREA | JSON: buttons in the map pin tooltip popup |
| Mass Action Layout | `maps__MassActionLayout__c` | TEXTAREA | JSON: buttons in the mass action toolbar |
| POI Layout | `maps__POILayout__c` | TEXTAREA | JSON: buttons for point-of-interest markers |
| Custom Address Layout | `maps__CustomAddressLayout__c` | TEXTAREA | JSON: buttons for custom address markers |
| My Position Layout | `maps__MyPositionLayout__c` | TEXTAREA | JSON: buttons shown at current user location |

---

## maps__MapsSettings__c — Maps Setting

Org-wide configuration record for Salesforce Maps. API name: `maps__mapssetting__c`.

> Single-record settings object. Controls geocoding provider, routing defaults, live tracking, and UI behavior across the org.

---

## maps__DataLayerSetting__c — Maps Data Layer Setting

API name: `maps__datalayersetting__c`. Configures external or Salesforce-sourced data layers (e.g., ArcGIS feature layers, CSV imports). Paired with `maps__Layer__c`.

---

## maps__MarkerLayerComponent__c — Maps Marker Layer Component

API name: `maps__markerlayercomponent__c`. Stores a sub-query component for a `maps__MarkerLayer__c` when `maps__QueryComponents__c > 0`. Each record holds one partial SOQL clause that is merged into the full layer query.

---

## maps__MarkerLayerRelatedList__c — Maps Marker Layer Related List

API name: `maps__markerlayerrelatedlist__c`. Defines a related-list sub-query attached to a `maps__MarkerLayer__c`. Enables showing child records (e.g., Opportunities on Account pins) in the tooltip or side panel.

---

## maps__MinimapSetting__c — Maps MiniMap Setting

API name: `maps__minimapsetting__c`. Controls the embedded mini-map component (the small map widget on record pages). Settings include default zoom, layers to display, and whether the component is enabled per object type.

---

## maps__DebugLog__c — Maps Debug Log

API name: `maps__debuglog__c`. System-written log records for geocoding, routing, and live-tracking operations. Used for troubleshooting failed geocode batches and async job errors.

---

## maps__ScheduledJob__c — Maps Scheduled Job

API name: `maps__scheduledjob__c`. Tracks Apex scheduled jobs triggered by Maps (e.g., nightly geocoding batch, live-asset sync). Fields include job ID, status, last run time, and next run time.

---

## maps__Analytic__c — Maps Analytic

API name: `maps__analytic__c`. Stores a saved analytics/reporting configuration for use in the Maps analytics panel. References a `maps__MarkerLayer__c` and defines metric aggregation settings.

---

## maps__DriveProfile__c — Maps Drive Profile

API name: `maps__driveprofile__c`. Defines vehicle or travel constraints for routing (e.g., truck height restrictions, avoid toll roads). Referenced by advanced route settings to influence route calculation.

---

## maps__Route__c — Maps Route

API name: `maps__route__c`. Represents a saved or optimized route. Stores route metadata (date, owner, total distance, duration) and links to `maps__Waypoint__c` children.

---

## maps__Waypoint__c — Maps Waypoint

API name: `maps__waypoint__c`. A single stop on a `maps__Route__c`. Stores the linked Salesforce record, sequence order, scheduled arrival/departure times, and geocoordinates.

---

## maps__SettingsGroup__c — Maps Settings Group

API name: `maps__settingsgroup__c`. Groups a set of Maps configuration settings (layer defaults, geocoding profiles, routing preferences) for assignment to users or profiles via `maps__SettingsGroupAssignment__c`.

---

## maps__SettingsGroupAssignment__c — Settings Group Assignment

API name: `maps__settingsgroupassignment__c`. Junction object between `maps__SettingsGroup__c` and a User or Profile. Determines which settings group applies to a given user.

---

## maps__Click2Create__c — Maps Click-2-Create Setting

API name: `maps__click2create__c`. Configures the Click-to-Create feature, which allows users to create a Salesforce record by clicking a point on the map. Fields include target object API name, default field values, and the record creation form layout.

---

## maps__ArcGISBatchPushSetting__c — Maps ArcGIS Batch Push Setting

API name: `maps__arcgisbatchpushsetting__c`. Defines a scheduled batch push of Salesforce record data to an ArcGIS feature service. Stores the ArcGIS service URL, authentication, field mappings, and schedule.

---

## maps__ArcGISBatchPushLog__c — Maps ArcGIS Batch Push Log

API name: `maps__arcgisbatchpushlog__c`. System-written result record for each ArcGIS batch push run. Stores record counts, errors, and run timestamp. Child of `maps__ArcGISBatchPushSetting__c`.

---

## maps__ArcGISBatchPushTest__c — Maps ArcGIS Batch Push Test

API name: `maps__arcgisbatchpushtest__c`. Stores the result of a test/dry-run push to ArcGIS before committing a live batch. Used to validate field mappings and connectivity.

---

## maps__AssignmentPlan__c — Maps Assignment Plan

API name: `maps__assignmentplan__c`. Defines an automated record-assignment plan: which SObject to assign, the geographic or rule-based criteria, and the target user/queue. Run via `maps__AssignmentPlanRun__c`.

---

## maps__AssignmentPlanRun__c — Maps Assignment Plan Run

API name: `maps__assignmentplanrun__c`. Represents a single execution of a `maps__AssignmentPlan__c`. Stores run status, record counts assigned, and any errors encountered.

---

## maps__AssignmentRule__c — Maps Assignment Rule

API name: `maps__assignmentrule__c`. A single rule within an `maps__AssignmentPlan__c`. Defines the filter condition and the target owner to assign matching records to.

---

## maps__CalendarResourceShift__c — Maps Calendar Resource Shift

API name: `maps__calendarresourceshift__c`. Represents a time-blocked shift for a field resource in the Maps scheduling calendar. Stores resource ID, shift start/end times, and territory.

---

## maps__CalEventBaseObj__c — Maps Calendar Event Base Object

API name: `maps__caleventbaseobj__c`. Configuration record that registers a Salesforce object as a calendar event source for Maps scheduling. Similar in role to `maps__BaseObject__c` but for calendar/event display.

---

## maps__CalEventLookup__c — Maps Calendar Event Lookup

API name: `maps__caleventlookup__c`. Maps a field on a calendar event source object to a display property (e.g., subject, start time, end time) used by the Maps scheduling calendar.

---

## Advanced Routing Objects

The following objects are part of the Salesforce Maps Advanced Routing module (requires the routing add-on).

### maps__AdvRoute__c — Maps Advanced Route

API name: `maps__advroute__c`. The parent record for an advanced (optimized) route. Stores overall route parameters, status, and links to template, waypoints, and output.

### maps__AdvRouteContinuousTravel__c — Maps Advanced Route Continuous Travel

API name: `maps__advroutecontinuoustravel__c`. Configures multi-day continuous travel settings for an advanced route (e.g., overnight travel allowed between stops).

### maps__AdvRouteDataSet__c — Maps Advanced Route Data Set

API name: `maps__advroutedataset__c`. Defines the set of Salesforce records (stops) to include in an advanced route optimization run. References `maps__AdvRouteTemplate__c`.

### maps__AdvRouteDefaultValue__c — Maps Advanced Default Value

API name: `maps__advroutedefaultvalue__c`. A single default field value applied to records created or updated during an advanced route run. Child of `maps__AdvRouteDefaultValuesGrouping__c`.

### maps__AdvRouteDefaultValuesGrouping__c — Maps Advanced Default Values Group

API name: `maps__advroutedefaultvaluesgrouping__c`. Groups a set of `maps__AdvRouteDefaultValue__c` records for bulk application during routing output.

### maps__AdvRouteOffDays__c — Maps Advanced Route Off Days

API name: `maps__advrouteoffdays__c`. Stores non-working days (holidays, blackout dates) for a resource used in advanced routing.

### maps__AdvRouteOutputType__c — Maps Advanced Route Output Type

API name: `maps__advrouteoutputtype__c`. Defines what the routing engine should output (e.g., update existing records, create new activity records, generate a report).

### maps__AdvRoutePriority__c — Maps Advanced Route Priority

API name: `maps__advroutepriority__c`. Assigns a priority score or tier to records within an advanced route data set, influencing which stops are scheduled first.

### maps__AdvRoutePromotionalPeriodConfiguration__c — Maps Advanced Promotional Period Config

API name: `maps__advroutepromotionalperiodconfiguration__c`. Configures a promotional period window where routing rules or visit frequencies differ from the standard schedule.

### maps__AdvRouteQueryFilter__c — Maps Advanced Route Query Filter

API name: `maps__advroutequeryfilter__c`. A single filter condition applied when querying records for an advanced route data set. Child of `maps__AdvRouteQueryFilterGrouping__c`.

### maps__AdvRouteQueryFilterGrouping__c — Maps Advanced Route Query Filter Group

API name: `maps__advroutequeryfiltergrouping__c`. Groups multiple `maps__AdvRouteQueryFilter__c` records with AND/OR logic to form the full record-selection criteria for a route data set.

### maps__AdvRouteRoutableObject__c — Maps Advanced Route Routable Object

API name: `maps__advrouteroutableobject__c`. Registers a Salesforce object as eligible for inclusion in advanced routing. Stores field mappings (address, duration, time windows) needed by the routing engine.

### maps__AdvRouteSettings__c — Maps Advanced Route Settings

API name: `maps__advroutesettings__c`. Stores global and per-template optimization parameters: max stops per route, travel mode, start/end location, optimization objective (minimize time vs. distance).

### maps__AdvRouteTemplate__c — Maps Advanced Route Template

API name: `maps__advroutetemplate__c`. A reusable template that predefines routing settings, data sets, and output configuration. Users run routing jobs from a template.

### maps__AdvRouteTemplateUser__c — Maps Advanced Route Template User

API name: `maps__advroutetemplateuser__c`. Junction object granting a specific user access to a `maps__AdvRouteTemplate__c`.

### maps__AdvRouteVisitWindows__c — Maps Advanced Route Visit Windows

API name: `maps__advroutevisitwindows__c`. Defines the acceptable arrival time window(s) for a stop in an advanced route (e.g., must arrive between 9 AM–11 AM).

### maps__AdvRouteWaypoint__c — Maps Advanced Route Waypoint

API name: `maps__advroutewaypoint__c`. A stop within an `maps__AdvRoute__c` run result. Stores optimized sequence, scheduled arrival time, linked Salesforce record, and drive time from prior stop.

---

## Live Tracking Objects

The following objects support Salesforce Maps Live (real-time asset/driver tracking). Requires the Live add-on.

### maps__LiveAsset__c — Maps Live Asset

API name: `maps__liveasset__c`. Represents a tracked vehicle or mobile asset. Stores current GPS position, last-seen timestamp, speed, heading, and link to the assigned driver.

### maps__LiveAssetDailySummary__c — Maps Live Asset Daily Summary

API name: `maps__liveassetdailysummary__c`. Aggregated daily statistics for a live asset: total distance, drive time, stop count, and mileage by state. One record per asset per day.

### maps__LiveAssetDailySummaryEvent__c — Maps Live Asset Daily Summary Event

API name: `maps__liveassetdailysummaryevent__c`. Individual events within a daily summary (e.g., start of shift, stop, speeding event). Child of `maps__LiveAssetDailySummary__c`.

### maps__LiveAsyncError__c — Maps Live Async Error

API name: `maps__liveasyncerror__c`. Records errors from asynchronous Live processing jobs (GPS data ingestion, telematics webhooks). Used for diagnostics and retry logic.

### maps__LiveDailyDriveTrip__c — Maps Live Daily Drive Trip

API name: `maps__livedailydrivetrip__c`. Represents a single continuous drive trip within a day. Stores start/end coordinates, timestamps, distance, and linked asset.

### maps__LiveDailySummaryConfig__c — Maps Live Daily Summary Configuration

API name: `maps__livedailysummaryconfig__c`. Controls how daily summaries are computed: which events count as stops, minimum stop duration, mileage rounding rules.

### maps__LiveDriver__c — Maps Live Driver

API name: `maps__livedriver__c`. Links a Salesforce User to a tracked live asset. Stores driver ID, license information, and assignment status.

### maps__LiveDriverAssignment__c — Maps Live Driver Assignment

API name: `maps__livedriverassignment__c`. Tracks the history of which driver was assigned to which asset and during what time period.

### maps__LiveEvent__c — Maps Live Event

API name: `maps__liveevent__c`. A single telemetry event from a tracked asset (location ping, ignition on/off, speeding alert, geofence entry/exit). Core raw-data object for Live.

### maps__LiveEventAssociation__c — Maps Live Event Association

API name: `maps__liveeventassociation__c`. Links a `maps__LiveEvent__c` to a Salesforce record (e.g., associates a location ping with a nearby account or check-in activity).

### maps__LiveHolidayRelationship__c — Maps Live Holiday Relationship

API name: `maps__liveholidayrelationship__c`. Associates a Live working-hours configuration with a holiday calendar, so scheduled hours automatically account for holidays.

### maps__LiveHolidayTime__c — Maps Live Holiday Time

API name: `maps__liveholidaytime__c`. Stores a specific holiday date and its effect on live asset working-hours calculations (full day off, reduced hours, etc.).

### maps__LiveIoTDevice__c — Maps Live IoT Device

API name: `maps__liveiotdevice__c`. Represents a physical IoT/telematics device (GPS tracker, OBD dongle) installed in a vehicle. Stores device ID, make/model, and connection status.

### maps__LiveSummaryStateMileage__c — Maps Live Summary State Mileage

API name: `maps__livesummarystatemileage__c`. Breaks down daily mileage by US state (or region) for IFTA (fuel tax) reporting. Child of `maps__LiveAssetDailySummary__c`.

### maps__LiveSummaryStopRelationshipConfig__c — Maps Live Summary Stop Config

API name: `maps__livesummarystoprelationshipconfig__c`. Configures how stop events in a daily summary are matched to Salesforce records (accounts, contacts) for automatic check-in creation.

### maps__LiveWorkingHours__c — Maps Live Working Hours

API name: `maps__liveworkinghours__c`. Defines the expected working-hours schedule for a live asset or driver (days of week, start/end times). Used to filter out after-hours pings from summaries.

---

## Territory Planning Objects

The following objects support Salesforce Maps Territory Planning. Requires the Territory Planning add-on.

### maps__TerritoryAggregation__c — Territory Aggregation

API name: `maps__territoryaggregation__c`. Represents a geographic aggregation unit (e.g., a county, zip code cluster, or custom region) within a territory planning data set.

### maps__TerritoryAggregationSetting__c — Territory Aggregation Setting

API name: `maps__territoryaggregationsetting__c`. Configuration for how territory aggregation units are built: source geography type, aggregation metric (accounts, revenue, population), and thresholds.

### maps__TerritoryNode__c — Territory Node

API name: `maps__territorynode__c`. A single geographic unit (e.g., zip code, county) that can be assigned to a territory. The atomic building block for territory planning alignments.

### maps__TPAlignment__c — Territory Planning Alignment

API name: `maps__tpalignment__c`. The top-level record for a territory planning project/alignment. Contains the name, status, and links to all areas, roles, and assignments within the plan.

### maps__TPArea__c — Territory Planning Area

API name: `maps__tparea__c`. A named territory (sales territory, service zone) within an alignment. Groups `maps__TerritoryNode__c` records and is assigned to a `maps__TPRole__c`.

### maps__TPComment__c — Territory Planning Comment

API name: `maps__tpcomment__c`. A stakeholder comment attached to a `maps__TPAlignment__c` or `maps__TPArea__c`. Supports review/approval workflows for territory changes.

### maps__TPDataSet__c — Territory Planning Data Set

API name: `maps__tpdataset__c`. Defines the Salesforce data (object, fields, filters) used to measure territory balance and performance within an alignment (e.g., account revenue by zip code).

### maps__TPHistory__c — Alignment History Record

API name: `maps__tphistory__c`. Audit record capturing changes to a territory alignment over time (area reassignments, boundary edits). Enables before/after comparison.

### maps__TPRole__c — Territory Planning Role

API name: `maps__tprole__c`. Represents a role tier in the territory hierarchy (e.g., Regional VP → District Manager → Territory Rep). Determines how `maps__TPArea__c` records are structured.

### maps__TPUnitAssignment__c — Territory Planning Unit Assignment

API name: `maps__tpunitassignment__c`. Assigns a `maps__TerritoryNode__c` to a `maps__TPArea__c`. The junction record that defines which geographic units belong to which territory.

### maps__TPUserPreference__c — Territory Planning User Preference

API name: `maps__tpuserpreference__c`. Stores per-user UI preferences for the Territory Planning interface (saved filters, map view state, default alignment).

---

## Part 2 — `SFMaps__` Namespace Objects (Routing Module)

These objects are installed as part of the Salesforce Maps routing module. They may not be present in all orgs. Verify with `SELECT Id FROM SFMaps__Route__c LIMIT 1` before referencing.

> **Key distinction:** `maps__Route__c` (Part 1) is a Maps Advanced Route Waypoint template used in automated visit planning. `SFMaps__Route__c` (below) is the daily field route object used by standard Maps routing.

---

## SFMaps__Route__c

One record per day per rep. Represents a field rep's planned route for a single day.

| Field | API Name | Type | Notes |
|---|---|---|---|
| Name | `Name` | Text | Auto-generated or rep-named |
| Date | `SFMaps__Date__c` | Date | The day this route is planned for |
| User | `SFMaps__User__c` | Lookup(User) | The field rep |
| Status | `SFMaps__Status__c` | Picklist | `Draft`, `In Progress`, `Completed`, `Cancelled` |
| Start Address | `SFMaps__StartAddress__c` | Text | Route start point |
| End Address | `SFMaps__EndAddress__c` | Text | Route end point (can match start) |
| Total Distance | `SFMaps__TotalDistance__c` | Number | Miles or km (unit set in org prefs) |
| Total Duration | `SFMaps__TotalDuration__c` | Number | Estimated minutes for entire route |
| Optimized | `SFMaps__Optimized__c` | Checkbox | Whether Maps has optimized stop order |

---

## SFMaps__RouteStop__c

An individual stop on a `SFMaps__Route__c`. Links to any geocoded Salesforce record.

| Field | API Name | Type | Notes |
|---|---|---|---|
| Route | `SFMaps__Route__c` | Lookup | Parent route |
| Stop Order | `SFMaps__StopOrder__c` | Number | Sequence position |
| Record | `SFMaps__Stop__c` | Lookup (polymorphic) | The Salesforce record being visited |
| Status | `SFMaps__Status__c` | Picklist | `Planned`, `Completed`, `Skipped`, `Cancelled` |
| Scheduled Arrival | `SFMaps__ScheduledArrival__c` | DateTime | Calculated ETA |
| Actual Arrival | `SFMaps__ActualArrival__c` | DateTime | Set on check-in |
| Notes | `SFMaps__Notes__c` | TextArea | Rep notes for the stop |
| Duration | `SFMaps__Duration__c` | Number | Planned visit duration (minutes) |

---

## SFMaps__CheckIn__c

GPS-verified visit log created when a rep checks in at a route stop or an unplanned location.

| Field | API Name | Type | Notes |
|---|---|---|---|
| User | `SFMaps__User__c` | Lookup(User) | Who checked in |
| Route Stop | `SFMaps__RouteStop__c` | Lookup | Parent route stop (if from a route) |
| Record | `SFMaps__Record__c` | Lookup (polymorphic) | The Salesforce record visited |
| Check-In Time | `SFMaps__CheckInTime__c` | DateTime | GPS-verified arrival timestamp |
| Latitude | `SFMaps__Latitude__c` | Number | GPS latitude at check-in |
| Longitude | `SFMaps__Longitude__c` | Number | GPS longitude at check-in |
| Distance from Record | `SFMaps__DistanceFromRecord__c` | Number | Feet between rep GPS and record address |
| Activity Created | `SFMaps__ActivityCreated__c` | Checkbox | Whether a Task/Event was auto-created |

---

## SFMaps__DataLayer__c

Routing-module configuration for a Salesforce object displayed as a map layer. Distinct from `maps__MarkerLayer__c` (the core layer object).

| Field | API Name | Type | Notes |
|---|---|---|---|
| Name | `Name` | Text | Layer display name in the Maps panel |
| Object API Name | `SFMaps__ObjectApiName__c` | Text | e.g., `Account`, `Lead` |
| Label Field | `SFMaps__LabelField__c` | Text | Field API name for the pin tooltip |
| Color Field | `SFMaps__ColorField__c` | Text | Field used for color-coding pins |
| Max Records | `SFMaps__MaxRecords__c` | Number | Default 5,000 |
| Active | `SFMaps__Active__c` | Checkbox | |
| Shared | `SFMaps__Shared__c` | Checkbox | If true, all Maps users see this layer |

---

## SFMaps__Marker__c

A custom point-of-interest pinned on the map. Not tied to a Salesforce record — used for ad-hoc annotations.

---

## SFMaps__Waypoint__c (Routing Module)

Address-only intermediate point on a `SFMaps__Route__c`. Influences driving path but does not generate check-ins or activities. Distinct from `maps__AdvRouteWaypoint__c` (Maps Advanced automated routing waypoint).

---

## SFMaps__LayerFilter__c

Filter criteria record associated with a `SFMaps__DataLayer__c`. Stores individual SOQL-style WHERE conditions.

---

## Object Disambiguation Table

| Concept | Object to Use | Namespace |
|---|---|---|
| Geocoding config for an SObject | `maps__BaseObject__c` | maps__ |
| Saved marker layer shown on map | `maps__MarkerLayer__c` | maps__ |
| Third-party / external data layer | `maps__Layer__c` | maps__ |
| Shape / boundary layer | `maps__ShapeLayer__c` | maps__ |
| Rep's daily field route | `SFMaps__Route__c` | SFMaps__ |
| Stop on a daily route | `SFMaps__RouteStop__c` | SFMaps__ |
| GPS visit log | `SFMaps__CheckIn__c` | SFMaps__ |
| Routing module data layer config | `SFMaps__DataLayer__c` | SFMaps__ |
| Ad-hoc map annotation pin | `SFMaps__Marker__c` | SFMaps__ |
| Maps Advanced visit plan route | `maps__AdvRoute__c` | maps__ |
| Maps Advanced route waypoint | `maps__AdvRouteWaypoint__c` | maps__ |
