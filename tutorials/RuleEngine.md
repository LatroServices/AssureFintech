# Rule Engine
The Rule Engine provides the system user with front-end capabilites to configure logical, query-driven, and threshold-based controls and fraud transaction monitoring rules. 

## Creating a New Rule
1. Go to the Assure Fintech portal and log in to your account
2. Go to Engines > Rule Engine page
3. On the *Rule Engine page*, select *Create Rule* button. 
4. On the *Query Builder page*, select the *Add Source* button
5. Enter a Rule Name
6. Select the Runtime interval
  a. Runtime interval = how frequently the rule will refresh/update.
5. Select the rule *Window* 
  a. Window = The aggregation period in which the rule conditions will be applied to the selected data source.
6.Select the *Source*
7. Add new conditions by selecting *+RULE*
9. Group condition statements by selecting *+GROUP*
10. Apply the *AND* or *OR* statement by using the main drop-down
11. To add in multiple data sources, use the *+ ADD SOURCE* button.
12. To save the rule, use the *+ADD* button in the upper right corner of the page.

## Viewing & Editing Existing Rules
1. Go to Engines > Rule Engine to view a complete list of all existing rules on the system.
2. Click on an existing rule to view the configuration details.
3. Once viewing an existing rules config details you can directly make any updates to the rule.
4. To save the changes, use the *UPDATE* button inn the uppoer right corner of the page. 

## Detection Results
There are two methods of access the detections page. 
- Method 1: Go to Engines > Rule Engine and click *View All Detections* button
- Method 2:
-     Go to Engines > Rule Engine and select the desired rule.
-     From the desired rule's configuration page, select the *View Detections* button in the upper right corner.

Once you are on the *Detections* page
1. Apply any desired filters to view the detection results.
2. Use the *Download Detections* button to export the filtered list of detections to a CSV file.

## Investigating Detections
There are several different tools withi the Assure Fintech Web App that serve as investigation tools to further analyze suspect wallets and transaction events. 
- See [Transaction Browser](./TransactionBrowser.md)
- See Wallet Profiles (coming soon)

## Creating Alarms for Detections
To configure alarm notifications for a given Rule, see the [Alarms Tutorial](./Alarms.md)
