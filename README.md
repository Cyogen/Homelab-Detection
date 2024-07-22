# Homelab - Detection

## Objective

Goal is gain practical experience in SIEM and log analysis using Splunk within a Windows 11 environment. 
Through hands-on exercises and simulations I will have the ability to collect, analyze, and respond to security events.

### Skills Learned

- SIEM - SIEM concepts and principles & Splunk configuration.
- Logging - Windows log configuration.
- Log analysis - Splunk queries & search interface to analyze logs.
- Detection rules - Develop detection rules and use cases for identifying security incidents.
- Splunk alerts - Configuring Splunk alerts with specific detection rules.

### Tools Used

- Splunk Enterprise (trial)
- Windows 11 Professional
- pfSense

## Notes

Splunk's UI:
-  Data Sources:  *Settings* > *Data Inputs*.  This will list various data input methods. Clicking into each one will show an overview of the data sources being utilized.
-  Data (Events): *Search & Reporting* app allows a quick scan through data using the *FAST* mode. *VERBOSE* mode  allows event details and raw event data.  All fields extracted from it will be shown.
-  Fields: *Selected Fields* always shown in the events.  *Interesting Fields* appear in at least 20% of events.
-  Data Models: *Settings* > *Data Models* under the *Knowledge* section.  Provides organized hierarchical views of data.
    a. Clicking on a Data Model will open the Editor.  The data is divided into objects.  The objects contain fields.
   
## SPL 
1.  Find the account name with the highest amount of Kerberos authentication ticket requests:

![image](https://github.com/user-attachments/assets/a45b5fbc-a931-49d3-885a-5507a8683269)
    -  index=*:  Search should include all indexes so no data is excluded.<br>
    -  sourcetype="WinEventLog:Security" : Restricts the search events to Windows Security event logs.<br>
    -  EventCode=4768 :   This one I had to search before getting the right one. Kerberos auth uses ticket requests.  The first step in the process has a client requesting a ticket.<br>
    -  | stats count by Account_Name :  stats is used to generate summary statistics.  Seeing results based on account name greatly simplifies the output.<br>
    -  | sort -count : Learned this one early in the process.  Results are sorted in order based on the totals.  *-count* shows it in a descending order. <br>

2.  Sort through Event Codes 4624 to find the amount of times a distinct computer was accessed by the "SYSTEM" account.

![image](https://github.com/user-attachments/assets/019b51b1-2476-48d5-a169-35a0d87b0eae)


3. The most challenging of all SPL queries I've had to come up with was a search against all 4624 events to find the account name that made the most login attempts within a span of 10 minutes.
![image](https://github.com/user-attachments/assets/84fb8b07-0e18-4c15-8c7b-fe6667423e2f)

    While the index=* and EvenCode=4624 are expected, finding a way to group and sort 10 minute time spans was not as intuitive as I had hoped.<br>
   *stats min(_time) as first_login* finds the earliest logon time for each account.<br>
   *max(_time) as last_login* finds the latest logon time for each account.<br>
   *count as login_attempts* counts the total number of logon attempts for each account.<br>


