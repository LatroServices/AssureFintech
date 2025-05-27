## Version 1.1 [20-May-2025]
This product release is focused on Rating, Reconcilation engines, and creating alarms for controls

### Main Updates
- Added [Rate Management](../tutorials/RateManagement.md)
- Added [Data Feeds ](../tutorials/DataFeeds.md)
- Added [Control List Page](../tutorials/ControlList.md)
- Added [Reconciliation Engine](../tutorials/ReconciliationEngine.md)
- Added [Alarms Summary](../tutorials/alarmsummary.md)
- Updated [Alarms Configuration](../tutorials/Alarmsconfig.md) to support control-based alarms
- Updated [Alarms Event List](../tutorials/Eventlist.md) with additional data fields and filters 

### Other Updates


### Bug Fixes
- Changed all date pickers and other datetime places to use UTC instead of browser localtime
- Fixed critical vulnerability bugs identified during vulnerability testing
- Unified all tables to use new custom table as the control drill down
- Updated bulk upload in rate management to use single API hit instead of multiple API hits
- Fized filtering and sorting that was not working for some table fields in control drill down
- Formated Transaction Count as integer in transaction reports
- Fixed strange behavior in date picker (closing unexpectedly)
- Improved text indicator visuals (font color)
- Labeled date instead of Category for exported CSV in control drill down


### Known Bugs
- N/A
