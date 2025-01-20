# Alarms gFeature Overview

## Business Goal

The primary purpose of the Alarms feature is to serve as a notification mechanism that continuously monitors and detects specific conditions based on defined profiles. It watches detections from various sources, such as Rules Engines and Control Lists, and triggers notifications to the user when a profile is matched.

## Key Features

1. **Notification System**: 
   - The alarms feature will send notifications outside of the core system for selected profiles.
2. **Automated Actions**:
   - Enables automated responses based on alarm outputs or detections, enhancing user engagement and proactive responses.

## Architecture

The Alarms service is designed as a microservice in a standalone container, allowing seamless integration with other tools.

![image](https://github.com/user-attachments/assets/7df5408a-23b0-4823-a9b5-47946fd9fa99)


### Alarms Configs Page
This page allows users to create/edit alarms.

1. **Alarm Lifecycle**:
   - Once created, an alarm should continuously monitor new detections and send notifications for profile matches.
   - Alarms can be Enabled/Disabled.
   - When Alarm Clarification is changed, previous detections will be flagged and archived as SOFT DELETE, but they wont be considered by any alrgorithm or displayed in alarm detections.
2. **Detection Lifecycle**:
   - Define the handling of detections when an alarm configuration changes.
   - The Alarm will only notify users when certain conditions are met during specificed time interval.
   - Alarms should only monitor current detections when enabled, with each alarm run considered an isolated instance, meaning it won't report any previous labeled detections.
3. **Access Control**:
   - While all users will be able to view and edit alarms.
4. **Email Notification**:
   - Email notifications will only work if the server contians internet. Otherwise each new notification will always be available on demand.

### Alarms List Page
This page allows users to observe the notifications list and can view the detections for each alarm.

## Alarm Lifecycle

### Startup

During initialization, the scheduler will check if there are any enabled alarms that already exist from alarm_configs from the Database. and immediately start monitoring them and executing their detection queries. Once the initialization is over, the Scheduler will constantly monitor the enabled alarms and run the detection queries every Time Window. If the alarm conditions are true, then a new detection is created and added to alarm_detections (Detection time, window and records) and alarm_notifications (Alarm Name, Alarm Type and priority, Detection time and count, and window).

![Screenshot of Alarm Service](./assets/Alarm_service_flow_V2.png)

### Alarm Creation

An Alarm is created by configuring the following: rule type to be monitored, Condition for Alarm (as of now, only Detection Count), the operator (<, <=, =, >, >=) and condition value, and finally, the window time/interval for each detection to work on. In addition, You can choose whether an alarm is enabled and if you want notifications.

List of alarm configurations:
- Alarm Name
- Description
- Rule Type
- Condition (Detection Count)
- Operation
- Value
- Window Time/Interval

### Alarm Handling

Once a new alarm is created, the Alarm Manager starts handling it by first adding the new alarm config to alarm_configs inside the Database. Alarm Manager calls the Scheduler which starts monitoring the newly added alarms immediately. In addition, it also adds the Alarm to a fixed schedule for monitoring.

### Alarm Detection

When the Scheduler finds an enabled alarm, it runs a query that finds all occurrences of a rule during the specified timeframe, if the condition matches then a new detection is created and added to the database in alarm_detections as well as alarm_notifications for email notification purposes.

Data stored in alarm_detections:
- Alarm ID
- Detection time
- Window
- Records

Data stored in alarm_notifications:
- Alarm ID
- Alarm Name
- Rule Type
- Priority
- Detection Time
- Detection Count
- Detection ID (from alarm_detections)
- Notification Time
- Window

### Detection Flow
![Screenshot of Detection Flow](./assets/detection_process_v2.png)

### Repo Env Variables
```bash
MONGODB_URI=<MONGODB_URL>
PORT=3000   # change based on deployment setup
ALARMS_DB_NAME=Alarms
ALARM_CONFIGS_COLLECTION=alarm_configs
ALARM_DETECTIONS_COLLECTION=alarm_detections
ALARM_NOTIFICATIONS_COLLECTION=alarm_notifications
ALARM_CONDITIONS_COLLECTION=alarm_conditions
NODE_ENV=development   # or production 
JWT_SECRET=<TOKEN>
```

### Collections Schema

- alarm_conditions 
``` bash
{
  "condition_name": "Detection Count",
  "deleted": false,
  "source_db": "Data",
  "source_collection": "detections",
  "is_unique": false
}
```
- alarm_configs
``` bash
{
  "alarm_name": "New Alarm Unique",
  "description": "Test Test Test",
  "type": "Rule",
  "type_option": "Large Cash Out",
  "rule_id": "6743723f1e851519ca22a8e3",
  "priority": "Low",
  "email_notification": "No",
  "date_created": {
    "$date": "2025-01-08T09:51:02.830Z"
  },
  "created_by": "Ali Bikawi",
  "enabled": true,
  "catchup_enabled": true,
  "time_window": 31680,
  "source_collection": "detections",
  "source_db": "Data",
  "query_config": {
    "mongo_query": {
      "rule_id": {
        "$oid": "6743723f1e851519ca22a8e3"
      }
    }
  },
  "status": {
    "status": "Open",
    "changed_by": "Ali Bikawi",
    "change_time": {
      "$date": "2025-01-08T11:17:26.503Z"
    },
    "closing_comments": ""
  },
  "last_run_time": {
    "$date": "2025-01-12T07:47:27.156Z"
  },
  "last_run_status": null,
  "last_error": null,
  "execution_metadata": {
    "total_runs": 0,
    "total_detections": 0,
    "last_detection_time": null
  },
  "condition_name": "Unique Detection Count",
  "is_unique": true,
  "operation": ">=",
  "condition_value": "10",
  "includeTable": false,
  "email_subject": "",
  "email_to": "",
  "email_cc": ""
}
```
## Run the microservice

### Production Mode
```bash
docker-compose up --build
docker-compose up alarms-service
```
Starts only the alarms service. Use when connecting to an external MongoDB.

### Development Mode
```bash
docker-compose --profile dev up
```
Starts both the alarms service and a local MongoDB instance for development/testing.

### Common Commands

#### Stop Containers
```bash
docker-compose down
```

#### Stop and Remove Volumes
```bash
docker-compose down -v
```

#### View Logs
```bash
docker-compose logs -f
```


#### Run Scripts
```bash
docker exec -it <dockerid> sh
node scripts/generateTestToken.js
```
