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
    a.  index=*:  Search should include all indexes so no data is excluded.
    b.  sourcetype="WinEventLog:Security" : Restricts the search events to Windows Security event logs.  This makes it the data source.
    c.  EventCode=4768 :   This one I had to search before getting the right one. Kerberos auth uses ticket requests.  The first step in the process has a client requesting a ticket.
    d.  | stats count by Account_Name :  stats is used to generate summary statistics.  Seeing results based on account name greatly simplifies the output.
    e.  | sort -count : Learned this one eaerly in the process.  Results are sorted in order based on the totals.  -count shows it in a descending order. 

2.  
