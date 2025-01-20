<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Alarms Feature Ohverview</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/github-markdown-css/5.1.0/github-markdown.min.css">
    <style>
        body {
            font-family: Arial, sans-serif;
        }
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        .screenshot {
            max-width: 100%;
            height: auto;
        }
        pre {
            background-color: #f6f8fa;
            border-radius: 6px;
            padding: 16px;
            overflow: auto;
        }
    </style>
</head>
<body>
    <div class="container markdown-body">
        <h1 id="alarms-feature-overview">Alarms Feature Overview</h1>

        <h2 id="business-goal">Business Goal</h2>
        <p>The primary purpose of the Alarms feature is to serve as a notification mechanism that continuously monitors and detects specific conditions based on defined profiles. It watches detections from various sources, such as Rules Engines and Control Lists, and triggers notifications to the user when a profile is matched.</p>

        <h2 id="key-features">Key Features</h2>
        <ol>
            <li><strong>Notification System</strong>
                <ul>
                    <li>The alarms feature will send notifications outside of the core system for selected profiles.</li>
                </ul>
            </li>
            <li><strong>Automated Actions</strong>
                <ul>
                    <li>Enables automated responses based on alarm outputs or detections, enhancing user engagement and proactive responses.</li>
                </ul>
            </li>
        </ol>

        <h2 id="architecture">Architecture</h2>
        <p>The Alarms service is designed as a microservice in a standalone container, allowing seamless integration with other tools.</p>
        <img src="https://github.com/user-attachments/assets/7df5408a-23b0-4823-a9b5-47946fd9fa99" alt="image" class="screenshot">

        <h3 id="alarms-configs-page">Alarms Configs Page</h3>
        <p>This page allows users to create/edit alarms.</p>
        <ol>
            <li><strong>Alarm Lifecycle:</strong>
                <ul>
                    <li>Once created, an alarm should continuously monitor new detections and send notifications for profile matches.</li>
                    <li>Alarms can be Enabled/Disabled.</li>
                    <li>When Alarm Clarification is changed, previous detections will be flagged and archived as SOFT DELETE, but they won't be considered by any algorithm or displayed in alarm detections.</li>
                </ul>
            </li>
            <li><strong>Detection Lifecycle:</strong>
                <ul>
                    <li>Define the handling of detections when an alarm configuration changes.</li>
                    <li>The Alarm will only notify users when certain conditions are met during a specified time interval.</li>
                    <li>Alarms should only monitor current detections when enabled, with each alarm run considered an isolated instance, meaning it won't report any previous labeled detections.</li>
                </ul>
            </li>
            <li><strong>Access Control:</strong>
                <ul>
                    <li>While all users will be able to view and edit alarms.</li>
                </ul>
            </li>
            <li><strong>Email Notification:</strong>
                <ul>
                    <li>Email notifications will only work if the server contains internet. Otherwise, each new notification will always be available on demand.</li>
                </ul>
            </li>
        </ol>

        <h3 id="alarms-list-page">Alarms List Page</h3>
        <p>This page allows users to observe the notifications list and can view the detections for each alarm.</p>

        <h2 id="alarm-lifecycle">Alarm Lifecycle</h2>
        <h3 id="startup">Startup</h3>
        <p>During initialization, the scheduler will check if there are any enabled alarms that already exist from alarm_configs in the Database and immediately start monitoring them and executing their detection queries. Once the initialization is over, the Scheduler will constantly monitor the enabled alarms and run the detection queries every Time Window. If the alarm conditions are true, then a new detection is created and added to alarm_detections (Detection time, window and records) and alarm_notifications (Alarm Name, Alarm Type and priority, Detection time and count, and window).</p>
        <img src="./assets/Alarm_service_flow_V2.png" alt="Screenshot of Alarm Service" class="screenshot">

        <h3 id="alarm-creation">Alarm Creation</h3>
        <p>An Alarm is created by configuring the following: rule type to be monitored, Condition for Alarm (as of now, only Detection Count), the operator (&lt;, &lt;=, =, &gt;, &gt;=) and condition value, and finally, the window time/interval for each detection to work on. In addition, You can choose whether an alarm is enabled and if you want notifications.</p>
        <ul>
            <li>Alarm Name</li>
            <li>Description</li>
            <li>Rule Type</li>
            <li>Condition (Detection Count)</li>
            <li>Operation</li>
            <li>Value</li>
            <li>Window Time/Interval</li>
        </ul>

        <h3 id="alarm-handling">Alarm Handling</h3>
        <p>Once a new alarm is created, the Alarm Manager starts handling it by first adding the new alarm config to alarm_configs inside the Database. Alarm Manager calls the Scheduler which starts monitoring the newly added alarms immediately. In addition, it also adds the Alarm to a fixed schedule for monitoring.</p>

        <h3 id="alarm-detection">Alarm Detection</h3>
        <p>When the Scheduler finds an enabled alarm, it runs a query that finds all occurrences of a rule during the specified timeframe. If the condition matches, then a new detection is created and added to the database in alarm_detections as well as alarm_notifications for email notification purposes.</p>

        <h4>Data stored in alarm_detections:</h4>
        <ul>
            <li>Alarm ID</li>
            <li>Detection time</li>
            <li>Window</li>
            <li>Records</li>
        </ul>

        <h4>Data stored in alarm_notifications:</h4>
        <ul>
            <li>Alarm ID</li>
            <li>Alarm Name</li>
            <li>Rule Type</li>
            <li>Priority</li>
            <li>Detection Time</li>
            <li>Detection Count</li>
            <li>Detection ID (from alarm_detections)</li>
            <li>Notification Time</li>
            <li>Window</li>
        </ul>

        <h3 id="detection-flow">Detection Flow</h3>
        <img src="./assets/detection_process_v2.png" alt="Screenshot of Detection Flow" class="screenshot">

        <h3 id="repo-env-variables">Repo Env Variables</h3>
        <pre>
MONGODB_URI=&lt;MONGODB_URL&gt;
PORT=3000   # change based on deployment setup
ALARMS_DB_NAME=Alarms
ALARM_CONFIGS_COLLECTION=alarm_configs
ALARM_DETECTIONS_COLLECTION=alarm_detections
ALARM_NOTIFICATIONS_COLLECTION=alarm_notifications
ALARM_CONDITIONS_COLLECTION=alarm_conditions
NODE_ENV=development   # or production 
JWT_SECRET=&lt;TOKEN&gt;
        </pre>

        <h3 id="collections-schema">Collections Schema</h3>

        <h4>alarm_conditions</h4>
        <pre>
{
  "condition_name": "Detection Count",
  "deleted": false,
  "source_db": "Data",
  "source_collection": "detections",
  "is_unique": false
}
        </pre>

        <h4>alarm_configs</h4>
        <pre>
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
        </pre>

        <h2 id="run-the-microservice">Run the microservice</h2>

        <h3 id="production-mode">Production Mode</h3>
        <pre>
docker-compose up --build
docker-compose up alarms-service
        </pre>
        <p>Starts only the alarms service. Use when connecting to an external MongoDB.</p>

        <h3 id="development-mode">Development Mode</h3>
        <pre>
docker-compose --profile dev up
        </pre>
        <p>Starts both the alarms service and a local MongoDB instance for development/testing.</p>

        <h3 id="common-commands">Common Commands</h3>

        <h4>Stop Containers</h4>
        <pre>
docker-compose down
        </pre>

        <h4>Stop and Remove Volumes</h4>
        <pre>
docker-compose down -v
        </pre>

        <h4>View Logs</h4>
        <pre>
docker-compose logs -f
        </pre>

        <h4>Run Scripts</h4>
        <pre>
docker exec -it &lt;dockerid&gt; sh
node scripts/generateTestToken.js
        </pre>
    </div>
</body>
</html>
