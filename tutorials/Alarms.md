# Alarm Config
The Alarms feature provides the user with the ability to configure different types of alarm notification events for detection events resulting from the Rule Engine. This section includes three core components relating to alarms: Configuration, Event List, and Summary. 

## Alarms: Configuration
### How to create a new alarm
1. Go to the Assure Fintech portal and log in to your account
2. Go to the Alarms > Configuration page
3. On the *Configuration page*, select the *Add Alarm* button
4. On the *Add Alarm* pop-up window, type the desired alarm *Name* and *Description*.
5. Use the *Rule Type* drop down menu to select the desired rule to create an alarm.
   *Note: only one rule can be configured per alarm*
6. Select the *Priority* level
7. Use the *Condition* drop down menu to select the desired alarm condition (rule detection count)
   a) Count of Unique Detections (unique detections events during the selected *Window Time* in step 10)
   b) Count of Total Detections (total detections events, including duplicates, during the selected *Window Time* in step 10)
8. Use the *Operation* drop down menu to select the operation value (<, <=, =, >=, >, !=) 
9. Type the condition *Value*
10. Select the *Window Time (in minutes)*, which corresponds to the alarm run/refresh interval.
11. Use the toggle button to *Enable* or *Disable* the alarm
    *Note: when enabled the alarm events will be available in the Web App*
13. Use the toggle button to set *Email Notifications* alerts
14. To save the alarm configuration, select *SAVE* 

### Viewing & Editing Existing Alarms
1. Go to Alarms > Configuration page to view a complete list of all existing alarms on the system.
2. Click the *Edit* button on an existing alarm to edit the configuration details.
3. To enable/disable an existing alarm, either:
   a) Use the toggle *Enable/Disable* button on the *Alarm Configuration* page
   b) Or click the *Edit* button on the existing alarm and use the toggle button to *Enable* or *Disable*

