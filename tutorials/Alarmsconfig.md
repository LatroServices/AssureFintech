# Alarm Configuration
The Alarms Configuration feature provides the user with the ability to create different types of alarm events for detection events resulting from the Rule Engine or breaches in the count of exception records for Controls. This Alarms feature realtes to three core components: Alarm Configuration, Alarm Event List, and Alarm Summary. 

## Alarms: Configuration
### How to Create a New Alarm

Follow the steps below to create a new alarm in the Assure Fintech platform:

1. **Log in** to your account via the **Assure Fintech portal**.
2. Navigate to **Alarms > Configuration**.
3. Click the **Add Alarm** button on the Configuration page.
4. In the **Add Alarm** pop-up window, select the alarm type:
   - `Rule` or `Control`
5. Enter the **Alarm Name** and **Description**.
6. Use the **Rule Type** radio buttons to select the alarm type.
7. Choose a **Condition** for how results will be evaluated:
   - `Count of Unique Detections` (distinct detection events during the selected window in step 10)
   - `Count of Total Detections` (all detection events including duplicates during the selected window in step 10)
   - `Count of Exception Records` (all exception records during the selected window in step 10)
   - *Note: Only one rule or control can be assigned per alarm.*
8. Choose the **Operation** to compare detection values (e.g., `<`, `<=`, `=`, `>=`, `>`, `!=`)
9. Enter the **Condition Value** (e.g., `> 10`)
10. Set the **Window Time** (in minutes) to determine how often the alarm is evaluated.
11. Set the **Priority** (e.g., High, Medium, Low).
12. Toggle to **Enable** or **Disable** the alarm.
13. Toggle **Email Notifications** if alert emails are required.
14. Click **SAVE** to complete the configuration.

---
### Viewing & Editing Existing Alarms
1. Go to **Alarms > Configuration** page to view a complete list of all existing alarms on the system.
2. Click the *Edit* button on an existing alarm to edit the configuration details.
3. To enable/disable an existing alarm, either:
   a) Use the toggle *Enable/Disable* button on the *Alarm Configuration* page
   b) Or click the *Edit* button on the existing alarm and use the toggle button to *Enable* or *Disable*

---

### Additional Notes

- On the **Alarm Configuration** page, you can filter alarms using:
  - `Date Created` ordering by (Desc/Asc)
  - `Created By`
  - `Alarm Type` (Rule or Control)
  - `Alarm Name`
  - `Priority`
  - `Email Notification` (Yes/No)
  - `Enabled/Disabled` status

- To review and monitor triggered alarms, click the **Event List** button at the top right of the page. This navigates you to the [Events list](../tutorials/Eventlist.md) , where you can view and manage all alarm events in detail.

