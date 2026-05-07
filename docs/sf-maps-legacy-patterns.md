# Salesforce Maps Legacy Patterns

Salesforce Maps was formerly known as **MapAnything**. These patterns were originally documented under that name. The `sma` Apex namespace and references to `/apex/sma__MapAnything` reflect the legacy product — the modern equivalent is `/apex/maps__maps` (see `skills/sf-maps-custom-actions/SKILL.md` for the current versions of all these patterns).

---

## Data Layer Comparison: Salesforce Maps vs. MapAnything

Salesforce Maps ships with 20 built-in data layers. Custom data layers are not supported. MapAnything had 40 data layers, many of which have been deprecated or consolidated.

### Layers in Salesforce Maps (20 total)

1. US Mile Markers
2. UK 2011 Census
3. Public & Private Schools (USA)
4. Property Data (USA)
5. New Zealand 2013 Census
6. Mexican Demographics (2010)
7. Hospitals
8. EU Census 2011
9. Colleges & Universities
10. Business Data (USA)
11. Business Data (UK)
12. Business Data (Canada)
13. Auto Dealerships
14. Airports
15. 2018 Energy Rate Data, United States
16. 2016 ABS Census, Australia
17. 2013 ACS 5yr, United States
18. 2011 National Household Survey, Canada
19. 2011 Census, Canada
20. 2010 Census, United States

### MapAnything Layers — Included in Salesforce Maps?

| # | MapAnything Layer | In Salesforce Maps? |
|---|---|---|
| 1 | Weather Scores | No |
| 2 | Walkability Scores | No |
| 3 | Voting Places, New Zealand | No |
| 4 | US Mile Markers | Yes |
| 5 | UK 2011 Census | Yes |
| 6 | Tesla Superchargers | No |
| 7 | Real Estate: Foreclosure, Inventory & Sales | No |
| 8 | Public & Private Schools | Yes |
| 9 | Property Data (USA) | Yes |
| 10 | Nielsen PRIZM | No |
| 11 | New Zealand 2013 Census | Yes |
| 12 | Mexican Demographics (2010) | Yes |
| 13 | MapAnything Property Data Sample | No |
| 14 | MapAnything Property Data | No |
| 15 | MapAnything Property + Solar Data (2018) | No |
| 16 | MapAnything Property + Solar Data | No |
| 17 | MapAnything Property + Loan Data | No |
| 18 | MapAnything Consumer Data | No |
| 19 | Hospitals | No |
| 20 | Global Points of Interest | No |
| 21 | EU Census 2011 | Yes |
| 22 | Demographics: Household, Population & Property | No |
| 23 | Crime Scores | No |
| 24 | Colleges & Universities | Yes |
| 25 | Business Data Sample | No |
| 26 | Business Data (USA) | Yes |
| 27 | Business Data (UK) | Yes |
| 28 | Business Data (Canada) | Yes |
| 29 | Auto Dealerships | Yes |
| 30 | Airports | Yes |
| 31 | 2016 Energy Rate Data, United States | Yes |
| 32 | 2016 ABS Census, Australia | Yes |
| 33 | 2015 Energy Rate Data, United States | No |
| 34 | 2013 ELSI Lunch Data, United States | No |
| 35 | 2013 ACS 5yr, United States | Yes |
| 36 | 2011 National Household Survey, Canada | Yes |
| 37 | 2011 Census, Canada | Yes |
| 38 | 2011 ABS Census, Australia | No |
| 39 | 2010 Census, United States | Yes |
| 40 | 2006 Census, Canada | No |

---

## Map It Button (Legacy `sma` Namespace)

The legacy MapAnything Map It button uses `/apex/sma__MapAnything` instead of `/apex/maps__maps`. The URL parameters are identical. For new implementations, use the current patterns in `skills/sf-maps-custom-actions/SKILL.md`.

**Simple — dynamic single record:**

```
{!
URLFOR( "/apex/sma__MapAnything",
      null,
      [recordId=Account.Id,
       baseObjectId="a034100000JEBY7AAP",
       tooltipField="Name",
       tooltipField2="OwnerId",
       zoom="16",
       color=URLENCODE("#66FF00")]
      )
}
```

**Simple — static single layer:**

```
{! URLFOR("/apex/sma__MapAnything", null, [layerId="a0O410000026nIE"]) }
```

**Intermediate — Map It with MA Assignment Rule shape layer:**

```
{!
 URLFOR( "/apex/sma__MapAnything",
         null,
         [baseObjectId="a034100000JEBY7AAP",
          recordId=Account.Id,
          tooltipField="Name",
          tooltipField2="OwnerId",
          tooltipField3="Type",
          tooltipField4="Phone",
          tooltipField5="Website",
          tooltipField6="Industry",
          tooltipField7="sma__MAAssignmentRule__r.sma__Territory__c",
          tooltipField8="Description",
          zoom="8",
          color=URLENCODE("#66FF00")
         ]
        )
}
```

**Advanced — Custom Settings–driven with full-screen toggle:**

```
{!
IF( TEXT(Account.Type) = "Customer",
    URLFOR( IF( $Setup.Account_Map_It_Button_Settings__c.Full_Screen__c,
                "/apex/sma__MapAnything?isdtp=vw",
                "/apex/sma__MapAnything"),
            null,
            [baseObjectId=$Setup.Account_Map_It_Button_Settings__c.Base_ObjectId__c,
             recordId=Account.Id,
             tooltipField="Name",
             tooltipField2="OwnerId",
             tooltipField3="Cloudbilt_CSM__c",
             tooltipField4="Customer_Segmentation__c",
             tooltipField5="CSM_Tier__c",
             tooltipField6="Owner.ManagerId",
             tooltipField7="sma__MAAssignmentRule__r.sma__Territory__c",
             tooltipField8="Description",
             zoom=$Setup.Account_Map_It_Button_Settings__c.Zoom__c,
             color=URLENCODE($Setup.Account_Map_It_Button_Settings__c.Color__c)]
          ),
    URLFOR( IF( $Setup.Account_Map_It_Button_Settings__c.Full_Screen__c,
                "/apex/sma__MapAnything?isdtp=vw",
                "/apex/sma__MapAnything"),
            null,
            [baseObjectId=$Setup.Account_Map_It_Button_Settings__c.Base_ObjectId__c,
             recordId=Account.Id,
             tooltipField="Name",
             tooltipField2="OwnerId",
             tooltipField3="Employee_Band_LinkedIn__c",
             tooltipField4="LinkedIn_Company_Search__c",
             tooltipField5="Website",
             tooltipField6="Linkedin__c",
             tooltipField7="sma__MAAssignmentRule__r.sma__Territory__c",
             tooltipField8="Description",
             zoom=$Setup.Account_Map_It_Button_Settings__c.Zoom__c,
             color=URLENCODE($Setup.Account_Map_It_Button_Settings__c.Color__c)]
          )
   )
}
```

**Relative URL examples:**

```
/apex/sma__MapAnything?layerid=a0O410000026nIE

/apex/sma__MapAnything?recordId=0014100000gXMTb&baseObjectId=a034100000JEBY7AAP&tooltipField=Name&tooltipField2=OwnerId&zoom=16&color=%2366FF00
```

---

## Show Popup Flow (Legacy — New Opportunity)

Opens a Salesforce Flow in a popup overlay from a MapAnything Custom Action. Action name: `New Opportunity`. Navigates to `/apex/c__NewOpportunityFlow`.

```xml
<apex:component >
    <apex:slds></apex:slds>
    <script type='text/javascript'
            src='https://cdn.rawgit.com/developerforce/Force.com-JavaScript-REST-Toolkit/4ab6f9c6f4e83f6f8db07fc545a5c01cbff8e50a/forcetk.js'>
    </script>

    <script type='text/javascript'>
        console.log('Start of New Opportunity Button');
        MAActionFramework.on('ready', function () {

            MAActionFramework.customActions['New Opportunity'].Action = "Javascript";
            MAActionFramework.customActions['New Opportunity'].events = {};
            MAActionFramework.customActions['New Opportunity'].ActionValue = function (options) {
                var recordIdArray = [];
                for (var i = 0; i < options.records.length; i++) {
                    recordIdArray.push(options.records[i].Id);
                }
                var recordIdString = recordIdArray.join(',');

                var baseUrl = '/apex/c__NewOpportunityFlow?&id=';
                // Note: calling /flow/... directly does not work — must use a Visualforce page wrapper
                var fullUrl = baseUrl + recordIdString;

                var popupHTML =
                    '<div>' +
                       '<iframe style="height: 100%; width: 100%" src="' + fullUrl + '&isdtp=vw" />' +
                    '</div>';

                MA.Popup.showMAPopup({
                    template: popupHTML,
                    width: 600,
                    height: 600,
                    popupId: 'customPopUpId',
                    title: 'New Opportunity',
                    subTitle: '',
                    buttons: [{ text: 'Close', type: 'slds-button_destructive', keepOpen: false }]
                });
            };

        }); // End legacy MAActionFramework ready function
    </script>
</apex:component>
```

---

## Custom Mass Action (Pass IDs to Apex Controller)

Passes selected record IDs to a Visualforce page via a GET request. The Apex controller reads them from the `ids` URL parameter.

**Custom Action setup:**
- Navigate to: Setup → Installed Packages → MapAnything → Configure → Settings → Custom Actions tab
- Request Type: GET
- Check **Add Records?**
- Parameter Name: `ids`
- URL: full URL to the Visualforce page (must include the instance)
- After saving, add the action to the Mass Action Layout in the Button Sets tab

**Expected URL format when triggered:**

```
https://yourorg--c.na51.visual.force.com/apex/MACustomMassAction?ids=0010V00002CbLfzQAF,0010V00002CbQflQAF,...
```

**Visualforce page:**

```xml
<apex:page controller='MACustomMassActionController' >
    <apex:pageBlock>
        <apex:pageMessages></apex:pageMessages>
    </apex:pageBlock>
</apex:page>
```

**Apex controller:**

```apex
public class MACustomMassActionController {
    private map<string,string> PageParameters = ApexPages.currentPage().getParameters();
    private set<string> RecordIds = new set<string>();

    public MACustomMassActionController() {
        if (PageParameters.containsKey('ids')) {
            RecordIds.addAll(PageParameters.get('ids').split(','));

            if (RecordIds.size() > 10000) {
                AddPageMessage(ApexPages.severity.ERROR, 'Max Number of Records is 10,000');
            } else {
                AddPageMessage(ApexPages.severity.Info, 'Number of Records Selected: ' + string.valueOf(RecordIds.size()));
            }
        }
    }

    public void AddPageMessage(ApexPages.severity Severity, string Summary) {
        ApexPages.addmessage(new ApexPages.message(Severity, Summary));
    }
}
```

---

## Single Record Button — Update Record

Updates a single record's field via ForceTK when clicked from a MapAnything Custom Action. Action name: `Submit for Conversion`. Updates `Lead.Status` to `'Submitted'`.

```xml
<apex:component >
    <script type='text/javascript'
            src='https://cdn.rawgit.com/developerforce/Force.com-JavaScript-REST-Toolkit/4ab6f9c6f4e83f6f8db07fc545a5c01cbff8e50a/forcetk.js'>
    </script>
    <script type='text/javascript'>
        var $createContactStatus;
        MAActionFramework.on('ready', function () {

            MAActionFramework.customActions['Submit for Conversion'].Action = "Javascript";
            MAActionFramework.customActions['Submit for Conversion'].events = {};
            MAActionFramework.customActions['Submit for Conversion'].ActionValue = function (options) {
                $createContactStatus = MAToastMessages.showLoading({
                    message: MASystem.Labels.MA_Loading + '...',
                    timeOut: 0,
                    extendedTimeOut: 0
                });
                var record = options.records[0];
                standardRecordUpdate(record);
            };

            function standardRecordUpdate(record) {
                var client = new forcetk.Client();
                client.setSessionToken(MA.SessionId);
                client.update(
                    'Lead',
                    record.Id,
                    { Status: 'Submitted' },
                    function (response) {
                        MAToastMessages.hideMessage($createContactStatus);
                        MAToastMessages.showSuccess({ message: 'Successfully submitted this Lead.' });
                    },
                    function (err) {
                        MAToastMessages.hideMessage($createContactStatus);
                        MAToastMessages.showError({
                            message: 'Saving Error',
                            subMessage: err,
                            timeOut: 0,
                            extendedTimeOut: 0,
                            closeButton: true
                        });
                    }
                );
            }

        }); // End legacy MAActionFramework ready function
    </script>
</apex:component>
```

---

## Single Record Button — Create Related Record

Creates a new Contact related to the selected Account record via ForceTK. Action name: `New Contact`.

```xml
<apex:component >
    <script type='text/javascript'
            src='https://cdn.rawgit.com/developerforce/Force.com-JavaScript-REST-Toolkit/4ab6f9c6f4e83f6f8db07fc545a5c01cbff8e50a/forcetk.js'>
    </script>
    <script type='text/javascript'>
        var $createContactStatus;
        MAActionFramework.on('ready', function () {

            MAActionFramework.customActions['New Contact'].Action = "Javascript";
            MAActionFramework.customActions['New Contact'].events = {};
            MAActionFramework.customActions['New Contact'].ActionValue = function (options) {
                $createContactStatus = MAToastMessages.showLoading({
                    message: MASystem.Labels.MA_Loading + '...',
                    timeOut: 0,
                    extendedTimeOut: 0
                });
                var record = options.records[0];
                standardRecordCreation(record);
            };

            function standardRecordCreation(record) {
                var client = new forcetk.Client();
                client.setSessionToken(MA.SessionId);
                client.create(
                    'Contact',
                    {
                        FirstName: 'Test',
                        LastName: 'Name',
                        AccountId: record.Id,
                        MailingCity: record.BillingCity,
                        MailingCountry: record.BillingCountry,
                        MailingPostalCode: record.BillingPostalCode,
                        MailingState: record.BillingState,
                        MailingStreet: record.BillingStreet
                    },
                    function (response) {
                        MAToastMessages.hideMessage($createContactStatus);
                        MAToastMessages.showSuccess({ message: 'Successfully created new Contact.' });
                    },
                    function (err) {
                        MAToastMessages.hideMessage($createContactStatus);
                        MAToastMessages.showError({
                            message: 'Saving Error',
                            subMessage: err,
                            timeOut: 0,
                            extendedTimeOut: 0,
                            closeButton: true
                        });
                    }
                );
            }

        }); // End legacy MAActionFramework ready function
    </script>
</apex:component>
```

---

## Associate Custom Drawn Shape Layer to an Opportunity

Saves a user-drawn polygon from the map as a territory and associates it with an Opportunity record in real time.

**Custom Action setup:**
- Title: `Save Territory To Opportunity` (must match the name in the component code)
- Modes: Desktop & Mobile
- Action: `Load page in new window`
- Request Type: GET
- Do not include Shape Info; do not Add Records
- Text area: `[Custom]`

**Opportunity Map It button** (to enter territory association mode):

```
/apex/sma__mapanything?territoryAssociation=true&recordId={!Opportunity.Id}&baseObjectId=a0bC000000TAtotIAD&tooltipField=Name&tooltipField2=dlrs_Num_of_Tasks__c&tooltipField3=Forecast_Percentage__c&zoom=16&color=%2366FF00
```

**Visualforce component** (registers the `Save Territory To Opportunity` handler):

```xml
<apex:component >
    <script type='text/javascript'>
        MAActionFramework.on('ready', function () {

            MAActionFramework.customActions['Save Territory To Opportunity'].Action = "Javascript";
            MAActionFramework.customActions['Save Territory To Opportunity'].events = {};
            MAActionFramework.customActions['Save Territory To Opportunity'].ActionValue = function (options) {
                if (p('territoryAssociation') && p('recordId')) {
                    getOpportunityRecord(p('recordId'));
                }
            };

            function getOpportunityRecord(oppRecordId) {
                var processData = {
                    ajaxResource: 'MATooltipAJAXResources',
                    action: 'do_query',
                    q: 'SELECT Id, Name FROM Opportunity WHERE Id =\'' + oppRecordId + '\' LIMIT 1'
                };

                Visualforce.remoting.Manager.invokeAction(MARemoting.processAJAXRequest,
                    processData,
                    function(response, event) {
                        if (event.status && response.success && response.records.length > 0) {
                            $.each(response.records, function (i, oppRecord) {
                                autoTerritorySave(oppRecord.Id, oppRecord.Name);
                            });
                        } else {
                            growlError($('#growl-wrapper'), 'Error getting Opportunity record info.', 4000);
                        }
                    }
                );
            }

            function autoTerritorySave(oppRecordId, oppRecordName) {
                if (ContextMenuClick.type == 'shape') {
                    var terrName = oppRecordName;
                    var shape = ContextMenuClick.target;
                    var tempData = { proximityType: 'Polygon', points: [], isCustom: true };
                    var points = shape.getPath();
                    for (var i = 0; i < points.getLength(); i++) {
                        var xy = points.getAt(i);
                        tempData.points.push({ lat: xy.lat(), lng: xy.lng() });
                    }

                    var colorOpts = {
                        fillColor: $('#CustomShapePopup .fillcolor').val(),
                        borderColor: $('#CustomShapePopup .bordercolor').val(),
                        fillOpacity: $('#CustomShapePopup .fillopacity').val(),
                        labelEnabled: $('#CustomShapePopup #custom-shapelayer-label-enabled').is(':checked'),
                        labelOverride: $('#CustomShapePopup .label-text-override-input').val(),
                        labelJustification: $('#CustomShapePopup #custom-shapelayer-label-justification').val(),
                        labelFontSize: $('#CustomShapePopup #custom-shapelayer-label-font-size').val(),
                        labelFontColor: $('#CustomShapePopup #custom-shapelayer-label-font-color').val(),
                        labelBGColor: $('#CustomShapePopup #custom-shapelayer-label-bg-color').val(),
                        labelBGOpacity: $('#CustomShapePopup #custom-shapelayer-label-bg-opacity').val()
                    };

                    var geometryToStringify = [{
                        Name: terrName + '-geometry0',
                        sma__Geometry__c: JSON.stringify(tempData)
                    }];

                    var processData = {
                        ajaxResource: 'MATerritoryAJAXResources',
                        action: 'saveBoundaryInfoV2',
                        serializedTerritory: JSON.stringify({
                            Id: null,
                            Name: terrName,
                            sma__Description__c: $('#CustomShapePopup .shape-description').val(),
                            sma__User__c: MA.CurrentUser.Id,
                            sma__Folder__c: null,
                            sma__Options__c: JSON.stringify({
                                country: 'USA',
                                advancedOptions: { calculateTerritoryAggregates: false, dissolveGeometry: true },
                                colorOptions: colorOpts
                            }),
                            sma__CustomGeometry__c: true,
                            Opportunity__c: oppRecordId
                        }),
                        serializedGeometry: JSON.stringify(geometryToStringify)
                    };

                    Visualforce.remoting.Manager.invokeAction(MARemoting.processAJAXRequest,
                        processData,
                        function(res, event) {
                            $('#CustomShapePopup .maPopupLoading').addClass('hidden');
                            if (event.status && res.success) {
                                MALayers.refreshFolder();
                                growlSuccess($('#growl-wrapper'), 'Successfully saved this shape.', 4000);
                                MACustomShapes.drawV2({ id: res.data.Id });
                            } else {
                                growlError($('#growl-wrapper'), 'Unable to save custom shape.', 4000);
                            }
                        },
                        { escape: false }
                    );
                }
            }

        }); // End legacy MAActionFramework ready function

        function p(name) {
            name = name.replace(/[\[]/, "\\[").replace(/[\]]/, "\\]");
            var regex = new RegExp("[\\?&]" + name + "=([^&#]*)");
            var results = regex.exec(location.search);
            return results == null ? "" : decodeURIComponent(results[1].replace(/\+/g, " "));
        }

    </script>
</apex:component>
```

**Enabling Custom Components** — navigate to:
`https://{MY_DOMAIN}--maps.visualforce.com/apex/CustomComponents`
Find the Visualforce component you created and check the box to enable it.

