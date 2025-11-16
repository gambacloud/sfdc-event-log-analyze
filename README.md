# sfdc-event-log-analyze
Salesforce Event Log – How to Download EventLogFile Records

This tool analyzes Event Monitoring logs exported from Salesforce, specifically the EventLogFile object.
To use the tool, you must download raw EventLogFile CSV (or GZIP) files directly from Salesforce.

This README explains how to download those files.

✅ What Are Event Log Files?

Salesforce Event Monitoring provides detailed logs of user activity.
Each log is stored as an EventLogFile record, containing a downloadable CSV (sometimes GZIP-compressed) with event-level data.

Each file = one “event type” on one day, for example:

URI — Page views (UI)

API — REST/SOAP queries

ApexExecution — Apex SOQL executed by the platform

ReportExport — Report exports

Login, Logout, BulkAPI, LightningError, and many more

Official docs:
📄 https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_event_log_file.htm

How to Download EventLogFile Data

You can download logs using any of the following methods:

1️⃣ Workbench (Easiest method)
Step 1 — Go to Workbench

https://workbench.developerforce.com/login.php

Log in with your Salesforce org.

Step 2 — Run SOQL Query

Navigate to:

Queries → SOQL Query

Use the following query:

SELECT Id, EventType, LogFile, LogDate
FROM EventLogFile
WHERE LogDate = YESTERDAY
AND EventType IN ('URI', 'API', 'ApexExecution', 'ReportExport')
ORDER BY LogDate DESC

Step 3 — Download

Workbench will show a LogFile download link.
Click it → You will receive:

a .csv file
or

a .csv.gz (GZIP) file

Your analyzer tool supports both formats.

2️⃣ Salesforce Developer Console
Step 1 — Open Developer Console

Inside Salesforce:
Gear Icon → Developer Console

Step 2 — Query Editor

Paste:

SELECT Id, EventType, LogFile, LogDate
FROM EventLogFile
WHERE LogDate = LAST_N_DAYS:7
ORDER BY LogDate DESC


Execute, then click the LogFile links to download each file.

Official Event Monitoring docs:
📄 https://help.salesforce.com/s/articleView?id=sf.security_event_monitoring.htm

3️⃣ Salesforce Event Monitoring Analytics App (for customers with Shield)

If your org uses Shield Event Monitoring, you can download logs from the Analytics app.

Docs:
📄 https://help.salesforce.com/s/articleView?id=sf.security_em_analytics_app_intro.htm

How:

Open the Event Monitoring Analytics App.

Go to Datasets → EventLogFile.

Export as CSV.

4️⃣ REST API (Advanced / Automation)

You can download logs programmatically.

Step 1 — Query available files
GET /services/data/v58.0/query/?q=SELECT+Id,EventType,LogDate,LogFile+FROM+EventLogFile

Step 2 — Download the file
GET <value of LogFile field>


Docs:
📄 https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_event_log_file_download.htm

Recommended Event Types for This Tool

Your analyzer works best with these Event Types:

EventType	Why it's useful
URI	Detects UI access to objects/records
API	Shows REST/SOAP/Integration queries
ApexExecution	Shows Apex SOQL accessing objects/fields
ReportExport	Identifies data pulled via reports

These event types contain:

User Id (USER_ID)

Object accessed (RESP_OBJECT_TYPE)

Field accessed (RESP_FIELD)

Timestamp

Query text (SOQL, URI, export description)

How to Prepare a File for the Analyzer

Download one or more files using any of the methods above.

Files may come as:

.csv

.csv.gz

The tool supports multiple EventTypes and any number of rows.

Drag & drop the file into the analyzer UI.

Map columns (User / Object / Field / Query).

Filter by User Id.

The tool will automatically:

Extract unique Objects

Extract unique Fields

Build Object → Fields mappings

Generate insights for per-user permissions

Useful Additional Salesforce Documentation
Event Types Reference

📄 https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/event_log_file_event_types.htm

EventLogFile Object Reference

📄 https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_eventlogfile.htm

Event Monitoring Overview

📄 https://help.salesforce.com/s/articleView?id=security_event_monitoring_intro.htm

Need sample data?

A 100-row sample EventLogFile CSV is included for testing.
