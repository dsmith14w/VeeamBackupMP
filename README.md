# Veeam Backup MP

## Session Info
*	Session time: 17:15 GMT/ 12.15 ET May 22, 2024
*	Will present live
*	Title: Veeam Backup monitoring for 2024
*	Description: Veeam Backup is widely deployed and used by customers of SCOM, yet there is no Management Pack from Veeam which can be a bit of a blind spot for SCOM. David Smith from Wellspan takes us through his new and free community MP for exactly this purpose. From alerts when jobs fail to backup server health, this MP has you covered.

## Dates/Milestones:
*	Draft slides: 17th April
*	Session pre-recording first draft: 24th April
*	Session pre-recording final recording: 8th May
*	Event itself: 22nd May 2024

## Files
* [Notes](SCOMathon2024.txt)  
* MP File [Custom.Veeam.Backups.xml](Custom.Veeam.Backups.xml)
* [MP Doc html file](SCOMathon2024-MPdoc.html)
* [MP Doc via Browser](http://htmlpreview.github.io/?https://github.com/dsmith14w/VeeamBackupMP/blob/main/SCOMathon2024-MPdoc.html)
* [MP xlsx doc](https://github.com/dsmith14w/VeeamBackupMP/blob/main/Custom.Veeam.Backups%2024.2.16.60%20Full%20Report.xlsx)

---

## WMI 
 - WMI queries to root/VeeamBS  
 - Get-WmiObject -Namespace root/VeeamBS -ComputerName $ComputerName -List  

### WMI Queries used in MP
* select * from Job  
* select * from RespoiitoryBase  
* select * from Respoiitory  
* select * from wmi_extension - For Security check with credentials (Get-WmiObject -Namespace "root\VeeamBS" -ClassName wmi_extension -ComputerName -Credential $creds)

### Types information
- Static_HostTypeInfo
   - Get-WmiObject -Namespace "root\VeeamBS" -ClassName  Static_HostTypeInfo -ComputerName "$ComputerName" |select Type, DisplayName  
- Static_JobTypeInfo
   - Get-WmiObject -Namespace "root\VeeamBS" -ClassName  Static_JobTypeInfo -ComputerName "$ComputerName" |select Type, DisplayName   
 - Static_ProxyTypeInfo
   - Get-WmiObject -Namespace "root\VeeamBS" -ClassName  Static_ProxyTypeInfo -ComputerName "$ComputerName" | select Type, DisplayName   
- Static_RepositoryTypeInfo
   - Get-WmiObject -Namespace "root\VeeamBS" -ClassName  Static_repositoryTypeInfo -ComputerName "$ComputerName" | select Type, TypeDisplayName  
- Static_ViProxyTransportType
   -  Get-WmiObject -Namespace "root\VeeamBS" -ClassName  Static_ViProxyTransportType -ComputerName "$ComputerName" | select Type, DisplayName   
---
**Job Types**
~~~
   -1: SureBackupJob  
    0: Backup Job *
    1: Replication Job   
    2: Backup Copy Job   
    8: Quick Migration Job  
   24: File to Tape Backup Job  
   28: Backup to Tape Job  
   51: Backup Copy Job  * (File Copy Jobs)  
   52: SQL Log Backup Job  
   54: Oracle Log Backup Job  
   60: Replication Job *  
  202: Failover Plan  
12000: Agent Job  
12002: Agent Policy  
12003: Agent Job     
   63: Simple Backup Copy Worker Job  
   65: Simple Backup Copy Job  
13000: NAS Backup Job   
13003: NAS Backup Copy  
 4000: linux agent job - swallowing
~~~
\* Items the MP discovers (0, 51, 60)
  - Get-WmiObject -Namespace "root\VeeamBS" -ClassName  Static_JobTypeInfo -ComputerName "$ComputerName" |select Type, DisplayName

## Slides
- Why we created our own MP  
	- At one time we had the Veeam Backup MP when it was inculded with what we purchased  
	- This changed and the solution was expensive  
	- The solution which included the MP included software we did not need  
	- Purchase of the MP only was not an option  
	   Monitoring of Veeam backups was needed in our organization  
- Method of development  
	- We reviewed the existing Veeam Backup MP  
	- We reviewed our organizations use of the product  
  - Discussion with Veeam Backup staff of what to monitor  
	- Veeam Backup staff created a list of items that they would like to be monitored  
	- Worked with the Veeam Backup team to design a custom MP to suite our needs  
- What we chose to discover and monitor  
	- The design needed to be flexible to add additional items as needed  
	- Classes and discoiveries bassed on services for applications installed (Registry)  
	- Veeam Backup SQL servers discovery by script -OR- Group based on server naming convention  
	- Classes and discoveries based on backup type for jobs (Script)  
	- Monitor Services for Backup Server, Enterprise Manager, Proxy Server, SQL server  
	- Monitor jobs for discovered job types  
	- Rule for credential failure  
- What's in the MP (Export HTML Doc from MP Studio)  
	- 9 Classes  
	- 9 Discoveries  
	- 27 Monitors  
	- 1 Rule  
	- 1 Folder  
	- 11 Views  
- Flexibility of MP  
	- Additional classes for other job types are easy to duplicate  
	- Additional discoveries for new job types are easy to duplicate  
	- Additional monitors for the new job types are easy to duplicat  
	- Existing Monitors are esay to override and change parameters as necessary  
	- Alerts for additional job types will require viewing the VeeamBackup event log to identify job success and failure  
- Understanding Veeam Backups (This could be presented during the demo)  
	- The Veeam Backup teams provided the information for the services to be monitored  
	- WMI is used to gather details about the backups (name space: root/VeeamBS)  
	- WMI retrieval may require credentials based on your organization policies  
	- Job retrieval script contains code for handling credentails (We used a vault)  
	- Retrieve a list of all WMI - Get-WmiObject -Namespace root/VeeamBS -ComputerName $ComputerName -List  
	- Queries use in MP  
		- select * from Job  
		- select * from RespoiitoryBase  
		- select * from Respoiitory  
		- select * from wmi_extension - For Security check with credentials (Get-WmiObject -Namespace "root\VeeamBS" -ClassName wmi_extension -ComputerName -Credential $creds)

