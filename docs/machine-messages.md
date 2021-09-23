---
sidebar_label: 'Machine Messages'
---

# z/OS LSAM Mtchine Messages

Certain conditions on the z/OS LSAM may prevent SAM from completing a task and a message is produced indicating the source or cause of the error. The SAM is designed so that errors or invalid resource conditions do not cause a total halt of schedule processing. After reporting the error, the SAM may retry processing and may again encounter the same error and report it.

To prevent the SAM from encountering the same error repeatedly, the job(s) involved may be placed "ON HOLD", or the SAM may be placed "ON HOLD", until the error is resolved. Some errors may be resolved without operator intervention, such as a job exclusion or inclusion that cannot be satisfied. When the proper condition is satisfied, the SAM is notified by XPS390 and this resolves the error. If a condition is assigned to a DEMAND run, there may be a long time before the condition is satisfied; the SAM encounters the error repeatedly until resolution. Some messages require operator analysis or resolution; others are informational only.

## Message Syntax

Al: OpCon/xps for z/OS messages are formatted with IBM standard message notation:

XPS*nnnc*

- **XPS** is the product identifier.
- **nnn** is the message reference number.
- **c** is a message type indicator.

+--------------+------------------------------------------------------+
| Message Type | Explanation                                          |
+:============:+======================================================+
| I            | The message is Informational only.                   |
+--------------+------------------------------------------------------+
| A            | The message indicates a possible or probable         |
|              | operator Action is required.                         |
+--------------+------------------------------------------------------+
| W            | -   The message is a Warning message.                |
|              | -   Some operation or process may need review.       |
+--------------+------------------------------------------------------+
| E            | -   The message indicates an Error condition.        |
|              | -   Corrective action is probable.                   |
+--------------+------------------------------------------------------+
| None         | -   If type is none of the above the message is      |
|              |     Informational and the type indicator represents  |
|              |     the MODULE that issued the message.              |
|              | -   B=XPSUBMIT                                       |
|              | -   T=XPSTATUS                                       |
|              | -   X=XPSPLEX                                        |
|              | -   V=XPSSUPV                                        |
|              | -   O=XPSYSOUT                                       |
|              | -   S=XPSERVER                                       |
+--------------+------------------------------------------------------+

: IBM Message Type Explanations

## Messages Displayed on the MVS SYSLOG by XPR390

+----------------------------------+----------------------------------+
| XPR390 Messages                  |                                  |
+==================================+==================================+
| Message                          | Description                      |
+----------------------------------+----------------------------------+
| XPR001I Restart: From *start     | Indicates a restart was          |
| step* \[to *end step*\]          | requested from step *start       | |                                  | step*, optionally ending in *end |
|                                  | step*.                           |
+----------------------------------+----------------------------------+
| XPR001E: STEPNAME MISMATCH       | -   Indicates the step name on   |
|                                  |     the starting step in the JCL |
|                                  |     does not match the name in   |
|                                  |     the restart request.         |
|                                  | -   The job is flushed.          |
+----------------------------------+----------------------------------+
| XPR005I *stepname compcode*      | Indicates *the stepname* was     |
| (SKIPPED FOR RESTART)            | bypassed for the restart, but    |
|                                  | *compcode* is simulated for JCL  |
|                                  | conditional processing.          |
+----------------------------------+----------------------------------+
| XPR010I DELETE *dataset name*    | -   Indicates *the dataset name* |
| (*volume*)                       |     is going to be created in    |
|                                  |     the step about to start, but |
| XPR010I UNCATLG *dataset name*   |     it is already cataloged.     |
| (*volume*)                       | -   OpCon/xps takes the          |
|                                  |     indicated action:            |
| XPR010I HDELETE *dataset name*   |     -   **DELETE:** the          |
| (MIGRAT)                         |         *dataset* is to be       |
|                                  |         removed from an online   |
| XPR010I RETAIN *dataset name*    |         DASD device.             |
| (*volume*)                       |                                  |
|                                  |     ```{=html}                   |
|                                  |     <!-- -->                     |
|                                  |     ```                          |
|                                  |     -   **UNCATLG:** the         |
|                                  |         *dataset* is on tape or  |
|                                  |         offline DASD, and is     |
|                                  |         simply un-cataloged.     |
|                                  |                                  |
|                                  |     ```{=html}                   |
|                                  |     <!-- -->                     |
|                                  |     ```                          |
|                                  |     -   **HDELETE:** the HSM     |
|                                  |         migrated *dataset* is to |
|                                  |         be deleted.              |
|                                  |                                  |
|                                  |     ```{=html}                   |
|                                  |     <!-- -->                     |
|                                  |     ```                          |
|                                  |     -   **RETAIN:** the          |
|                                  |         *dataset* matched the    |
|                                  |         XPR filter table, and no |
|                                  |         action is taken.         |
+----------------------------------+----------------------------------+
| XPR011I OPT=C *dataset.name \_   | -   The OPT=C\|R shows which GDG |
| +\_nnn*                          |     resolution option was used.  |
|                                  | -   Indicates the *dataset.name* |
| XPR011I OPT=R *dataset.name \_   |     shows the current generation |
| +\_nnn*                          |     from the catalog.            |
|                                  | -   The *+nnn* shows the         |
|                                  |     relative generation assigned |
|                                  |     to the current generation    |
|                                  |     for the restart.             |
+----------------------------------+----------------------------------+
| XPR060A - No PCB Available -     | Indicates XPRLIST was requested  |
| Activate LSAM                    | to update the filter table, but  |
|                                  | the LSAM has not been started,   |
|                                  | and no storage anchor block is   |
|                                  | available.                       |
+----------------------------------+----------------------------------+
| XPR101E - Invalid or missing     | Indicates a command was issued   |
| parms                            | to XPRLIST, but the command was  |
|                                  | not recognized.                  |
+----------------------------------+----------------------------------+
| XPR102E - NO XPSPARMS DD OR      | Indicates XPRLIST was requested  |
| MEMBER NOT FOUND                 | to load or reload the filter     |
|                                  | table, but either the XPSPARMS   |
|                                  | DD is not present, or the        |
|                                  | requested XPRLST*xx* member is   |
|                                  | not in the dataset.              |
+----------------------------------+----------------------------------+
| XPR105I - FILTER TABLE UPDATED   | Indicates the XPR filter table   |
|                                  | was updated.                     |
+----------------------------------+----------------------------------+
| XPR200I - No Filter Table\       | An XPRLIST DISPLAY was           |
|                                  | requested, but there is not      |
|                                  | filter table.                    |
+----------------------------------+----------------------------------+
| XPR200I - Filter table contents  | An XPRLIST DISPLAY was           |
|                                  | requested. This message is       |
|                                  | followed by the table contents.  |
+----------------------------------+----------------------------------+

: XPR390 Messages

### Messages Displayed on the MVS SYSLOG by XPS390

|Message|Description|
|--- |--- |
|XPS000W - Another LSAM is Already Running in this LPAR|Indicates the new duplicate LSAM is terminated.
                                    SMA recommends waiting for the prior LSAM to complete before starting its replacement.|
|XPS001I - OpCon/xps Vv.rr.mmll Initialized – [machineid]|Indicates OpCon/xps for z/OS has initialized storage queues and control blocks successfully.
                                    Machineid is the MACHINEID= definition in XPSPRMnn, or the SMF ID of this LPAR, and subsequently the schedule "Machine ID" for messages that is honored by this LSAM.|
|XPS002W - XPSPRMnn Set to Vendor Defaults|Indicates no XPSPRMnn member was located in the start parameter list or the parameter library.
                                    The resulting default parameter values can be viewed with the "F LSAM,PARMS" command.|
|XPS003I - Storage Recovery Attempt|Issued during back out or remove processing.|
|XPS003E - Error During Storage Allocation Storage Recovery Failed for queuename|Indicates an MVS FREEMAIN error was encountered.
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS004A - RUNPARM Not Initialized-Restart|Indicates the parms dataset was not located in the start parameter list or the parameter library and the operator answered "N" to the XPS101V operator request. Supply the Parms dataset and restart.|
|XPS005I - TCP/IP - IP Open (nnn.nnn.nnn.nnn) Port: pppp|Issued during LSAM IP network initialization to indicate the LPAR Host IP address (nnn.nnn.nnn.nnn) returned by the OMVS "GETHOSTID" request, and to indicate the IP port (pppp) on which XPS390 is listening for SAM messages.|
|XPS006E - [subroutine] Abend/Shutdown Detected|Indicates the subroutine encountered an error and the LSAM is systematically shutting down the remaining subtasks and closing SAM communications.
                                    The LSAM terminates and must be restarted.
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS007E - Error During Storage Allocation|Indicates an MVS GETMAIN error was encountered.
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS008E - Token Fetch Error RC=8|Indicates an MVS CSA queue locate error was encountered.
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS009I - System Shutdown Underway|This is a response to an F LSAM,SHUTDOWN request.
                                    Indicates the LSAM is systematically shutting down operational subtasks, stopping XPSPLEX and closing SAM communications.
                                    The LSAM terminates.|
|XPS010I - Subtask Shutdown Underway|This is a response to an internal/external shutdown request.
                                    This message may follow an XPS009I or XPS006E message.
                                    Indicates the LSAM is systematically shutting down operational subtasks and closing SAM communications.
                                    The LSAM terminates.|
|XPS011I - [Job|Message] Queue Cleared|This is a response to an F LSAM, CLEARQ operator command.
                                    Indicates all ongoing schedule tracking fails.
                                
                                Note: This command should only be issued under the supervision of SMA Support.|
|XPS012I - Storage Recovery Complete|This is a response to a GETMAIN failure.
                                    This message may follow an XPS003E message.
                                    Indicates the LSAM was successful in freeing all allocated CSA prior to termination.|
|XPS013E - INITAPI Error: XPSERVER tcpiptask|Indicates the XPSERVER subroutine could not establish an interface to TCP/IP on this LPAR.
                                    Ensure that TCP/IP is operating properly and that an OMVS segment is defined to your security facility for OPCON/XPS.
                                    Ensure that the TCPIP= parameter in XPSPRMnn denotes the proper task name for your environment.|
|XPS014I - PSAM Initialization - Server in XCF Mode|Issued at an LSAM startup to indicate that the XPSERVER subroutine does not attempt to open IP communications to the SAM.
                                    All communications to the schedule activity monitor is accomplished through the Sysplex Coupling Facility and the LPAR designated as the Sysplex primary LSAM.|
|XPS015A - XPS390 Awaiting Connection to SAM Server
                                XPS015O - XPSYS0 Awaiting Connection to UI Window|Indicates a temporary communication disconnect occurred between the LSAM and the SAM schedule system.
                                    The LSAM waits approximately 20 seconds and retries the IP Socket ACCEPT request.
                                    If this message persists and all communication protocols appear operational, contact your TCP/IP LAN Network Support Technician.
                                    The Type code "O" indicates the message was issued by XPSYSOUT.|
|XPS016I - XPS390 SAM Server Connection Established
                                XPS016O - XPSYS0 Window Connection Established|Indicates that the temporary disconnect was corrected, and the LSAM and the SAM are in communication.
                                    The Type code "O" indicates the message was issued by XPSYSOUT.
                                    This message normally follows message XPS015A.|
|XPS017W - Cannot Bind Port; Suspending Retry for 30 Seconds
                                XPS017O - Cannot Bind Port; Suspending Retry for 30 Seconds|Indicates a temporary communication disconnect occurred between the LSAM and the SAM schedule system.
                                    The LSAM waits approximately 30 seconds and retries the IP Port BIND request.
                                    This message may be a normal result of stopping and immediately restarting the LSAM.
                                    z/OS TCPIP often "hold(s)" access to a port after it is closed to ensure any buffered messages are completed before allowing the port to be reopened.
                                    If this message persists and all communication protocols appear operational, contact your TCP/IP LAN Network Support Technician.
                                    The Type code "O" indicates the message was issued by XPSYSOUT.|
|XPS018W - Cannot Obtain Socket; Suspending Retry for 30 Seconds
                                XPS018O - Cannot Obtain Socket; Suspending Retry for 30 Seconds|Indicates a temporary communication disconnect has occurred between the LSAM and the SAM schedule system.
                                    The LSAM waits approximately 30 seconds and retries the IP Socket connection.
                                    This message may be a normal result of stopping and immediately restarting the LSAM.
                                    z/OS TCPIP often "hold(s)" access to a port after close to ensure any buffered messages are completed before allowing the port to be reopened and a socket assigned.
                                    If this message persists and all communication protocols appear operational, contact your TCP/IP LAN Network Support Technician.
                                    The Type code "O" indicates the message was issued by XPSYSOUT.|
|XPS019E - Storage Recovery Failed for [queuename]|Indicates the MVS FREEMAIN error was encountered during a REMOVEX, REPEXIT or RESET command.
                                    The LSAM could not free storage for [queuename] that was assigned to the LSAM.
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS020W - Process CSA Will Change at Next IPL|Issued in response to an "F LSAM,PROCESS=nn" operator command.
                                    CSA storage is allocated at IPL to coincide with the expected process limit set in the LSAM/Machine environment. To understand the significance of this message, refer to CSA Storage Allocation.|
|XPS021I - Current Stored PARMS for [sysid]|Issued in response to an "F LSAM,parm=" operator command to indicate the status of all run time parameters after processing the last command.
                                    A list of current LSAM parameter settings follows this message.
                                    This message and the subsequent parameter list are also issued by an "F LSAM,PARMS" command or by executing the XPSAUDIT program.
                                    The PROCESSES display issued does not show a "Stored" Process of zero. Since this count is used to size CSA storage queues, only the SAM is advised of a process of ZERO.|
|XPS022W - No PARMS Updated|Indicates a parameter update request was invalid.
                                    No Parms are updated if any parameter in the request command is invalid.|
|XPS023I - Process Count Set to Zero|Indicates a Machine Process Count of "0000" has been sent to the SAM.This is equivalent to setting the machine to LIMITED.
                                    Issued in response to an "F LSAM,PROCESS=0" operator command. This command is particularly useful when a z/OS system QUIESCE is underway. It allows a pause in the job start traffic to this LPAR without detailed changes to the SAM schedule.
                                    If another parameter change is requested, the process count reverts to the value it had before the PROCESS=0 command.
                                    If the LSAM is shut down, the process count reverts to the value defined in the XPSPRMnn upon restart.|
|XPS024A - Parameter Invalid: [parm]|Issued if a parameter error occurs during the LSAM start-up. If this is issued the LSAM terminates.
                                    Indicates the parm is invalid in response to one of the following:
                                    A RUNPARM member load during start-up.
                                    An operator command (F LSAM,parm=value).
                                    An LSAM JCL Parm (PARM='parm=value').
                                    Correct and retry the operation.|
|XPS024E - Parameter Invalid: [parm]|This message is essentially the same XPS024E. They both indicate a parameter error.
                                    Issued when a user enters an invalid parameter after the LSAM is started. The LSAM continues to run, but the new parameter is rejected.|
|XPS025E - LOG Dyn Alloc Error R15=nnnn|Indicates the LSAMLOG dataset could not be allocated.
                                    This error may be due to a catalog error or an invalid GDG override in the OPCONnn Proc.
                                    The task terminates.
                                    Restart the task.
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS026E - Error Opening XPSLOG - Restart|This indicates an open request failed after successfully allocating the RECLOG DD.
                                    The task terminates.
                                    Restart the task.
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS027I - OpCon/xps LSAM Log Dataset Spin-Off Underway|Each midnight (00:00) and upon operator request (F LSAM,SPINLOG) and new generation of the OPCON/XPS RECLOG is created. Logging is suspended during the activity of de-allocating, closing, re-allocating and reopening the log.|
|XPS028I - Sysout Agent Ready on Port: nnnn - JESx|Indicates the Enterprise Manager Sysout View agent (XPSYSOUT) successfully opened the IP communication port (nnnn) for Sysout requests.
                                    JESx displays the SAPI subsystem from which sysouts is retrieved.|
|XPS029I - Stop Command Recognized|This is a response to an "F LSAM,STOP" or a "P LSAM" operator command.
                                    Indicates the LSAM is systematically shutting down operational subtasks and closing SAM communications.
                                    The LSAM terminates.|
|XPS030I - [ejobname] Adopted From ssss|Indicates a job normally executed on another LPAR being run on the current LPAR due to a Sysplex adoption of the target Machine-ID.
                                    To view the status of Sysplex Machine-IDs with respect to the current LPAR, issue command: F XPSPLEX,STATUS.|
|XPS031E - Invalid Jobname Bypassed: [jobname]|Indicates the Job Name is an invalid MVS construct.
                                    This message is usually due to a special character in the job name or other invalid usage.
                                    XPS390 automatically converts lowercase characters to uppercase prior to searching the defined library for the JCL or Event Name.|
|XPS032E - JCL Member Not Found: [jobname] in [library DDNAME]|Indicates the SAM schedule requested JCL for [jobname] be submitted from [DDNAME] and the LSAM could not locate such a member name.
                                    Correct and reschedule.|
|XPS033E - [jobname] Invalid JobCard - Correct|Indicates the SAM schedule requested JCL for [jobname] be submitted and the LSAM encountered an invalid MVS Job Card.
                                    Correct and reschedule.|
|XPS034E - JCL Library BLKSIZE Invalid|Indicates the Block Size of each JCL PDS library must be greater than 800.
                                    Contact your MVS system Programmer for assistance.
                                    Message XPS035S follows with the DDNAME of the library.|
|XPS035E - Library DDNAME Error: [ddname]|Indicates the SAM schedule requested JCL be submitted from [ddname] and the LSAM could not locate such a library or the library had a non-recoverable error.
                                    Correct and reschedule.|
|XPS036E – Allocation/Open Error on INTRDR|Indicates an XPSUBMIT subtask was unable to allocate an internal reader for JCL submission.
                                    Restart the LSAM.
                                    Contact your JES System Programmer and ensure that there are enough INTRDRs defined.
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS037I - jobname (J0000) Submitted From [library] as [job name]
                                XPS037I - taskname(S0000) Submitted as [job name]|Indicates that a successful Batch JCL submission has occurred.
                                    The scheduled job [job name] requested submission of [jobname] from DDNAME: [library].
                                    JES has assigned the job number (Jnnnn)
                                    - or -
                                    
                                    Indicates that a successful Started Task initiation has occurred.
                                    The scheduled job [job name] requested submission of [taskname]. MVS has assigned the task number (Snnnn).|
|XPS038E - [ddname] DD Error For REXX Job: [jobname]|Indicates the [ddname] requested on the SAM schedule record (or the default DD SYSEXEC) for a dynamic REXX job is not defined to the LSAM.
                                    Correct and reschedule.|
|XPS039E - Token/Symbolic Syntax Error: [jobname]|Indicates an invalid syntax for a &SYMBOLIC or @TOKEN entry in the detail schedule record for [jobname] was encountered.
                                    Correct and reschedule.|
|XPS040A - Auditor Has Reestablished EOQ Marker|Indicates that a tracking queue was not properly synchronized and automatic recovery succeeded.
                                    Contact SMA Support if this message persists.|
|XPS041W - Warning: Cannot Establish EOQ Marker|Indicates a queue format error cannot be recovered.
                                    Contact SMA Support if this message persists.|
|XPS042E - CRC Error Detected - Retrying Request|Indicates the Check Record hash did not agree with the one sent by the SAM.
                                    This is a probable temporary network error.
                                    The XPSERVER subroutine discards the related record and request that SAM resend the request.
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS043W - DSN Exit Storage Not Freed
                                XPS043W - U83 Exit Storage Not Freed
                                XPS043W - U84 Tracking Exit Storage Not Freed
                                XPS043W - UJV Exit Storage Not Freed 
                                XPS043W - USI Exit Storage Not Freed|Indicates a request was made to reload exits (F LSAM,REPEXIT) and the old storage area was not successfully freed.
                                    This condition is bypassed and the new exit is loaded; however, repeated messages of this type may indicate that storage fragmentation in ECSA is possible.|
|XPS044W - WTO Exit Storage Not Freed|Indicates a request was made to reload exits (F LSAM,REPEXIT) and the old storage area was not successfully freed.
                                    This condition is bypassed and the new exit is loaded; however, repeated messages of this type may indicate that storage fragmentation in ECSA is possible.|
|XPS045E - Allocation Error on Sysout DS|Indicates the XPSYSOUT Agent encountered an error while allocating a JES Sysout.
                                    The probable cause for this error is that the Sysout is no longer available on the output queue.|
|XPS046W - TCP/IP Timeout: LSAM Has Lost Contact with SMA Netcom Service.|Indicates the XPSERVER has timed out waiting on Netcom to respond.
                                    The LSAM continues to poll the SMA Server every 10 – 20 seconds.
                                    This may be caused by a network outage or Netcom shutdown.
                                    When communication is resumed, message XPS093I is issued.
                                    If this message persists and all communication protocols appear operational, contact SMA support.|
|XPS046O - TCP/IP Timeout - [process] Restarting.|Indicates the XPSAGENT has timed out waiting on IP Window Response to the [process] indicated.
                                    This is not critical communications.
                                    If this message persists and all communication protocols appear operational, contact SMA support.|
|XPS047I - [$event]|This is a response to a step condition code or DSN trigger.
                                    Indicates the MSGIN command [$event] was sent to the SAM.
                                    The condition or trigger was caused by the job issuing the message.|
|XPS048O - InitAPI Error: XPSAGENT tcpiptask|Indicates the XPSERVER module cannot establish an interface to OpenMVS API.
                                    This error may be due to an OMVS segment not being defined in your security product or a failure in TCP/IP.
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS049A - [eventname] Not in Event Table|This is a response to a step condition code or DSN trigger.
                                    Indicates the MSGIN command [eventname] was referenced but could not be located in the LPAR event table.
                                    The condition or trigger was caused by the job issuing the message.
                                    Inspect the Event Table and ensure that the event name is spelled correctly and the table entry is available to this LPAR.|
|XPS050E - XPSCOMM PARM Error|Indicates an invalid PARM= parameter was found on the XPSCOMM execute statement or the KEY=.|
|XPS051E - XPSCOMM Tracking Error|Indicates the XPSCOMM encountered an error attempting to update the tracking record of the job in question.
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS052I - XPSCOMM Processing Successful|Indicates that the requested tracking queue update was successfully accomplished by XPSCOMM.|
|XPS053A - Invalid Sysid for Adoption|This is a response to an F LSAM,ADOPTWKLD(ssss,nn) command.
                                    Indicates an invalid sysid (ssss) was requested.
                                    The current LPAR SMFID and those of currently operating LPARS are not valid.|
|XPS054A - Invalid Reply|Indicates the operator responded incorrectly to either the XPS112A or XPS113A message.
                                    Review these message descriptions for the proper reply syntax.|
|XPS055A - SYSPLEX Mode Not Supported|Indicates a Sysplex Only request was made (starting XPSPLEX, etc.) that was inconsistent the SYSPLEX parameter in XPSPRMnn.
                                    Ensure SYSPLEX=Y is coded in XPSPRMnn if you wish to enable Sysplex services in OpCon/xps.|
|XPS056I - XPSPLEX Already Running|Indicates an attempt was made to start another XPSPLEX task when one was already running.
                                    Only one XPSPLEX task is allowed.
                                    The newly started task terminates.|
|XPS057A - Invalid Command|Indicates an "F XPSPLEX,command" was entered and the command syntax was invalid.
                                    Refer to the syntax of the F XPSPLEX command for further details.|
|XPS058W - WTO Msg Tracking Exit NOT Loaded|Indicates an internal error has prevented message tracking exit XPSWTOEX from loading.
                                    Message XPS073E or XPS068A may precede this message with causes.
                                    The WTO exit is not optional.
                                    Besides providing message triggering for scheduled jobs, it also provides for Not Catlg error support and other internal OpCon/xps support features.
                                    Contact SMA Support if the reason for this message cannot be determined.|
|XPS059E - Failed to [Process Function] RC=nnnn|Indicates the [Process Function] is a critical internal operational function.
                                    The diagnostic information in this message must be passed to SMA Support as soon as possible.
                                    The OPCONnn task may not continue to function if this error persists.|
|XPS060E - No PCB Available for Restart - Activate LSAM|Indicates a control block format error cannot be recovered during initialization.
                                    Jobs that were "in process" during a system outage might not be properly reported to SAM.
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS061E – XPSU84 Foreign Job Tracking Error|Indicates the LSAM encountered an OpCon/xps job submitted from a different NJE/MAS or JES2 LPAR that is not in LSAM/PSAM configuration.
                                    The job was canceled.
                                    Check the schedule detail for this job and review the job "Machine ID" field.
                                    Check the JCL for SYSID routing information and class assignments.
                                    Contact your application programmer or Scheduler for further information.|
|XPS062I - XPSU84: Step Action Initiated|This is a TRACE Level 2 response.
                                    It is received from the IEFU84 exit when the OpCon/xps trace facility is active.
                                    The trace facility should not be active unless specifically requested by SMA Support.
                                    To eliminate these messages enter command "F LSAM.TRAC=N".|
|XPS063I - XPSU84: Step Tracking Initiated|This is a TRACE Level 2 response.
                                    It is received from the IEFU84 exit when the OpCon/xps trace facility is active.
                                    The trace facility should not be active unless specifically requested by SMA Support.
                                    To eliminate these messages enter command "F LSAM.TRAC=N".|
|XPS064I - XPSU84: Tracking Record Updated|This is a TRACE Level 2 response.
                                    It is received from the IEFU84 exit when the OpCon/xps trace facility is active.
                                    The trace facility should not be active unless specifically requested by SMA Support.
                                    To eliminate these messages enter command "F LSAM,TRACE=N".|
|XPS065E - XPSCOMM Event [eventname] Not Found|Indicates the Event Token coded in the $EVENT=[eventname] parameter in the XPSCOMM program was not located in the system Event Table.
                                    Contact your production control department.
                                    Request that the production control department inspect the Event Table and ensure that the event name is spelled correctly and the table entry is available to the LPAR on which the job ran.|
|XPS066E - [queuename] CSA Queue Full!|Indicates there is not enough space in the [queuename] for additional entries.
                                    Refer to the definition in your SETQUES= parameter in XPSPRMnn.
                                    Refer to current SETQUES settings using the "F LSAM,DISP=PARMS" command.
                                    Contact SMA Support immediately if the reason for this message is not obvious.|
|XPS067E - ADD/POST Error on Submit Request|Indicates the XPSERVER subroutine encountered an error attempting to update a tracking record.
                                    Contact SMA Support if this message persists.|
|XPS069I - LSAM Task Must Be Down Before XPSPLEX Shutdown|This is a response to and operator command to shut down XPSPLEX.
                                    Indicates OPCONnn is still executing.
                                    The OPCONnn task must be terminated before XPSPLEX can be shut down.
                                    To shut down BOTH tasks (OPCONnn and XPSPLEX) use the "F LSAM,SHUTDOWN" command.|
|XPS070I - Pre-run Condition(c) Check: [jobname] [OK|Failed] For Job: [taskname]|Indicates that the Task Pre-run condition was either met (OK) or failed (Failed) for the [jobname] in question.
                                    The Task Pre-run condition (c) can be:
                                    E=Executing.
                                    N=Not Executing.
                                    Refer to the schedule [jobname] detail for information.|
|XPS071I - Prerun Check: aa/nn Unit=[unitname] Rdy For [jobname] at hh.mm.ss|Indicates that the Task Pre-run tape unites needed (nn) for [unitname] was tested and aa units were found available.
                                    If the count in aa is equal to or greater than the number of units needed (nn), job [jobname] is submitted immediately; otherwise, the job is sent back to SAM with a "Pre-run Failed" condition and SAM reschedules the job at the interval set by SAM installation options.|
|XPS072I - Prerun DSN: [trigger.dsname.suffix] For [jobname] Triggered by [+] [taskname] at hh.mm.ss|Indicates that the Task Pre-run Data Set Name (DSN) ending with the 20-character suffix [trigger.dsname.suffix] has been accessed as required by the pre-run condition in [jobname].
                                    The executing job or task that triggered the DSNAME resource is denoted by [taskname].
                                    If [taskname] is preceded by a plus sign (+), the trigger was manually released, in which case the [taskname] is the TSO Userid responsible for setting the trigger's count to zero.|
|XPS073E - [Error Condition]|Indicates a Trigger or Tracking Exit did not load properly.
                                    One of the following could not load an interface to MVS:XPSWTOEX.XPSUJV.XPRUSI.XPSU84.XPSU83.
                                    Following a general description, each error condition is described in detail below.|
|XPS073E - BLDL Failed for XPS3U84|XPSU83|XPSUJV|XPRUSI|XPSWTOEX|Indicates an error occurred in an attempt to load one of the trigger exits (i.e., the LOADLIB DD statement in the OPCONnn Proc failed to return the member name).
                                    Check to ensure the STEPLIB or link list contains the module.
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS073E - U84|U83|UJV|USI|WTOX Length not in TTR|Indicates an error occurred in an attempt to load one of the trigger exits (i.e., the LOADLIB DD statement in the OPCONnn Proc return the member name but the member has zero records).
                                    Check to ensure the STEPLIB or link list contains the named module. Re-link these modules using the STAGE2 JCL if necessary.
                                    Contact SMA z/OS Support if this message persists or the reason for its issuance cannot be determined.|
|XPS073E - Fixed ESQA Getmain Failed: U84EX|U83EX|UJVEX|USIEX|WTOEX|Indicates an error occurred in an attempt to load one of the trigger exits (i.e., the extended storage was not available for the request).
                                    Approximately 2.5K is needed in extended SQA for each module.
                                    Contact your system programmer to research the amount of extended storage allocated.
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS073E - Explicit Load for XPSU84|XPSU83|XPSUJV|XPRUSI|XPSWTOEX Failed|Indicates an error occurred in an attempt to load one of the trigger exits (i.e., the LOAD Macro returned a nonzero return code).
                                    This error may be due to environmental factors.
                                    Retry the load request [F LSAM,REPEXIT].
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS073E - CSVDYNEX Add Failed for XPSU83|XPSU84|XPSUJV|XPRUSI|CNZ_WTOMDBEXIT|Indicates an error occurred in an attempt to load a dynamic exit into the exit list (i.e., the CSVDYNEX Macro returned a nonzero return code).
                                    Additional messages are issued to the z/OS console by the Dynamic Exit Service.
                                    Correct the problems indicated by the associated messages, then retry the load request [F LSAM,REPEXIT].
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS073E - XPS390 Not Initialized|Indicates XPSELOAD program cannot be executed "stand alone" without OPCON/XPS being initialized.
                                    Start the OPCONnn task to reload exits automatically.|
|XPS074I - Pre-run Skipped by SAM Request: [jobname]|Indicates that the OpCon/xps operator has manually requested that the associated job be submitted without pre-run resource checks.|
|XPS075E - DSN Table Expire Date: yy.ddd|Indicates that any DSN Trigger entries that have not been referenced since Julian date yy.ddd, are expired from the current tracking table.
                                    The entry is reinstated if a job has a pre-run requirement specified. This value fixed by architecture.|
|XPS075I - Trigger Table Expire Date: xx.xxx|Indicates the LSAM log has spun off and a copy of the dataset and message tables was written to a new log. During this logging action, the LSAM calculates and reports an expiration date in the system log.
                                    The LSAM reports the date in Julian format, with a two- digit year and three-digit day number.|
|XPS275I - EXPIRING: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|Indicates a trigger table entry has not been referenced since the Trigger Table Expiration Date. The LSAM deletes the table entry, logs the action, and reports it to the system log. Refer to Message XPS075I above.
                                    The variable portion of the message contains the dataset or message filter of the deleted entry.|
|XPS076E - Error Loading SMF Tracking Exits|Indicates an error occurred in an attempt to load an SMF exit.
                                    Message XPS073E should precede this message with more detailed problem information.
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS077I - [exitname] loaded from [library]|Indicates which library the exit loader loads from.
                                    The [exitname] is the name of the exit being loaded. Depending on where the exit module was found, the [library] is either STEPLIB or is the name of the active linklist set.|
|XPS077I - [exitname] loaded from [library]|Indicates which library the exit loader loads from.
                                    The [exitname] is the name of the exit being loaded. Depending on where the exit module was found, the [library] is either STEPLIB or is the name of the active linklist set.|
|XPS077I - XPSU83 File Trigger Exit Established|Indicates that the dynamic load for the XPS390 SMF exit for file triggering was successful.|
|XPS077I - XPSU84 Job Tracking Exit Established|Indicates that the dynamic load for the XPS390 SMF exit for job/step tracking was successful.|
|XPS077I - XPSUJV Job Init Exit Established|Indicates that the dynamic load for the XPS390 SMF exit for job submission tracking was successful.|
|XPS077I - XPRUSI Step Init Exit Established|Indicates that the dynamic load for the XPS390 dataset cleanup exit for restart was successful.|
|XPS079I - XPSWTOEX Msg Trigger Exit Established|Indicates the WTO exit was successfully established.|
|XPS080A - XPS390 Not Initialized|Indicates control block format error caused a temporary tracking failure.
                                    This message may be issued during a "RESET" command. If this message is received at any other time, contact SMA Support.|
|XPS081A - XPS390 Tracking Record not Available|Indicates control block format error caused a temporary tracking failure.
                                    This message may be issued during a "RESET" command. If this message is received at any other time, contact SMA Support.|
|XPS082E - XPS390 Dyn Alloc Error: R15=nn; DALERR=xxxxxxxx|Indicates an error occurred while attempting to allocate the SYSEXEC DD for REXX dynamic call support.
                                    Refer to associated MVS allocation messages, where nn is the return code from SVC99 and xxxxxxxx is the failing Text Unit.
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS083I - [DSN|WTO] Table Entry: [entrytext] Added|Deleted by [jobname] at hh.mm.ss {via ISPF}|Indicates that the LSAM or a TSO user (via ISPF) has added a new DSN to WTO trigger to the Sysplex.
                                    The [entrytext] is the dataset name or WTO Text of the trigger. Refer to the [jobname] on the appropriate OpCon/xps schedule to determine the actual resource this addition is to detect.
                                    The ISPF form of this message is issued for ISPF TSO User table updates to the DSN Trigger table only if SPFAUDIT=Y is coded in XPSPRMnn.
                                    This message is also issued for ISPF User deletes.|
|XPS084I - REXX:[execname] ID=[jobname] Executing|Indicates the REXX [execname] was successfully called by XPSEVENT after dynamic allocation of SYSEXEC, SYSTSPRT and SYSTSIN.
                                    This message does not indicate the REXX has successfully completed.|
|XPS085I - REXX:[execname] RCD=nnnn, ID=[jobname]|Indicates the REXX [execname] was completed via XPSEVENT.
                                    If the REXX routine set the EXIT (nn) code, then RCD contains the set value.|
|XPS086I - ADOPT Command Successful|Indicates the LSAM has processed the operators ADOPTWKLD command.
                                    To review the status of Machine ID processing on this LPAR enter the F XPSPLEX,STATUS command.|
|XPS087I - DROP Command Successful|Indicates the LSAM has processed the operators DROPWKLD command.
                                    To review the status of Machine ID processing on this LPAR enter the F XPSPLEX,STATUS command.|
|XPS088I - Command: command For: XXXXXXXXXXXX Executed|Indicates execution of the MVS Command [command] was requested by XPSEVENT.
                                    XPS390 does not check the outcome if operator commands; nevertheless, associated "Message Triggers" may monitor the results of any given command.|
|XPS089I - REXX:[rexxname] RCD=[rtncode], ID=[jobname]|Indicates the LSAM has successfully executed the REXX routine [rexxname] as scheduled by job [jobname].
                                    The return code [rtncode] from the REXX routine is passed back to the SAM.|
|XPS090A - XPCB Token Not Initialized|Indicates the LSAM is entering "Primary Initialization" and allocating CSA storage for the first time after a system IPL.
                                    If this message is received at any other time, contact SMA Support.|
|XPS091A - XPCB Control Block Not Initialized|Indicates a control block format error caused a temporary tracking failure.
                                    This message may be issued during a "RESET" command.
                                    If this message is received at any other time, contact SMA Support.|
|XPS092I - REXX:execname ID=XXXXXXXXXXXX Prerun Exec|Indicates the REXX [execname] was successfully called by XPSEVENT as requested by [ID] after dynamic allocation of SYSEXEC, SYSTSPRT and SYSTSIN.
                                    The message does not indicate the REXX has successfully completed.|
|XPS093I - OpCon/xps SAM Contact Reestablished|Indicates XPSERVER had timed out waiting on Netcom to respond (message XPS046 should have been pending).
                                    This condition is no longer a problem and normal communications have been reestablished.|
|XPS094E - Event: [eventname] Missing|Indicates the event name noted was referenced by a DSN trigger, but was not found in the Event Table.
                                    Review the DSN trigger definitions and create/rename the event name to match the name in the DSN trigger entry.|
|XPS095E - Event: [eventname] Missing|Indicates the event name noted was referenced by a WTO trigger, but was not found in the Event Table.
                                    Review the DSN trigger definitions and create/rename the event name to match the name in the WTO trigger entry.|
|XPS096I - Sysplex Member: [memberid] Active as [PSAM|LSAM]|Indicates the XPSPLEX task has recognized that the LPAR shown in Sysplex member [memberid] has joined the OpCon/xps Sysplex group as a PSAM or an LSAM.
                                    Only one LSAM can be allocated in any one Sysplex configuration.|
|XPS097I - XRF/CF Link for Grp: [groupname], Mbr: [memberid]|Indicates the XPSPLEX task has successfully joined the LPAR to the OpCon/xps Sysplex group [groupname] as member [memberid].|
|XPS098E - XRF/CF MSGX Error: [returncode]; [reasoncode]|Indicates XPSU84 has experienced an error or interruption in Sysplex communications to other OpCon/xps members and was unable to send job status information.
                                    Report this message to SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS099B - XRF/CF [errrtn] Error: [returncode]; [reasoncode]|Indicates XPSUBMIT has experienced an error or interruption in Sysplex communications to other OpCon/xps members.
                                    Report this message to SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS099E - XRF/CF [errrtn] Error: [returncode]; [reasoncode]|Indicates XPSERVER has experienced an error or interruption in Sysplex communications to other OpCon/xps members.
                                    Report this message to SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS099T - XRF/CF [errrtn] Error: [returncode]; [reasoncode]|Indicates XPSTATUS has experienced an error or interruption in Sysplex communications to other OpCon/xps members.
                                    Report this message to SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS099X - XRF/CF [errrtn] Error: [returncode]; [reasoncode]|Indicates XPSPLEX has experienced an error or interruption in Sysplex communications to other OpCon/xps members.
                                    Report this message to SMA Support if this message persists or the reason for its issuance cannot be determined.|
|XPS100I - [jobname]: Tracking Record Added.|Indicates the [jobname] has been added to the current AdHoc schedule due to a match on the TRACLASS or TRACMASK parameters in XPSPRMnn.|
|XPS101I - Step Cond Code IGNORED as Requested|Indicates the SAM schedule entry for this job requested that the job be considered "OK" even if a given step ended in a condition code equal to or higher than a defined value.
                                    The job continues to process but OpCon/xps tracking is halted and a "Step OK" message is sent to the SAM.
                                    Check the schedule detail for this job and review the step "Cond Code OK" field.
                                    Contact your application programmer or scheduler for further information.|
|XPS102I - Job Marked FAILED Due to Step Cond Code|Indicates the SAM schedule entry for this job requested that the job be considered a failure if a given step ended in a condition code equal to or higher than a defined value.
                                    The job continues to process but OpCon/xps tracking is halted and a "Job Fin Error" message is sent to SAM.
                                    Check the schedule detail for this job and review the step "Cond Code Fail" field.
                                    Contact your application programmer or scheduler for further information.|
|XPS103I - Job Marked COMPLETE Due to Step Cond Code|Indicates the SAM schedule entry for this job requested that the job be marked "Complete" if a given step ended in a condition code equal to or higher than a defined value.
                                    The job continues to process but OpCon/xps tracking is halted and a "Job Fin OK" message is sent to SAM.
                                    Check the schedule detail for this job and review the step "Cond Code Comp" field.
                                    Contact your application programmer or scheduler for further information.|
|XPS104I - Job TERMINATED Due to Step Cond Code|Indicates the SAM schedule entry for this job requested that the job be cancelled if a given step ended in a condition code equal to or higher than a defined value.
                                    That value was met or exceed by this job step.
                                    The job is terminated after the offending step.
                                    Check the schedule detail for this job and review the step "Cond Code Abort" field.
                                    Contact your application programmer or scheduler for further information.|
|XPS105A - Confirm Y/N is XPS390 to be REMOVED|RESET|CYCLED?|This is a response to an "F LSAM, REMOVEX" or "F LSAM,RESET=S|C" command.
                                    Prompts the operator is prompted for confirmation of the action.
                                    A Reply of "Y" causes all internal storage allocations to be freed and reallocated.
                                    For RESET=S or REMOVEX, all current data in the queues, except DSN and WTO trigger monitor entries, are lost.|
|XPS106I - DSN: [dsname] Has Triggered Event: [eventname]|Indicates an ACTIVE unscheduled DSN trigger for the Data Set [dsname] has been satisfied.
                                    The action defined in Event Table entry [eventname] is taken.
                                    To view the event detail, use the XPSPF001 TSO table interface command.|
|XPS107A - Confirm Y/N is Process Count to Remain at ZERO?|This prompt is issued during LSAM startup if the process count was zero when the LSAM was stopped.
                                    A reply of "Y" leaves the process count at "0".
                                    A reply of "N" reverts the process count to the last value defined by PROCESS= command or the value in XPSPRMnn.|
|XPS108I - Message from: [job/task name] Has Triggered Event: [eventname]|Indicates an ACTIVE unscheduled WTO message trigger was activated by job [job/task name].
                                    The action defined in Event Table entry [eventname] is taken.
                                    To view the event detail, use the XPSPF001 TSO table interface command.|
|XPS109E - XPSU84: Locate Error - Tracking Suspended: [schdkey]
                                 
                                XPS109E - [jobname]: Locate Error - Tracking Suspended|Indicates a tracking error was encountered.
                                    Contact SMA Support if this message persists or the reason for its issuance cannot be determined.
                                    A possible reason for this message may be that a scheduled job was awaiting execution when the system, or LPAR failed. After IPL, the LSAM task was not yet started before the JES initiators, and no tracking queue addresses match the original submission addresses. XPS390 allows the job to execute, but does not report status to SAM or may report a "failed" status. You may manually complete the job on SAM and continue processing.|
|XPS110A - RUNPARMS Not Initialized, specify XPSPARM=XX, Y to use SMA Defaults or N to cancel|Indicates that the LSAM is in "Primary Initialization" and no XPSPRMnn are available.
                                    If this message is received at any other time, contact SMA Support.
                                    XPSPRMnn can be provided through the XPSPRMnn DD statement in the OPCONnn Proc.
                                    If not specified in the JCL, the default member is XPSPRM00 for the default agent or XPSPRM0x for an agent with XPSID=x.
                                    Responding XPSPARM=xx will load the parms from XPSPRMxx.
                                    Responding with Y will initialize the agent with default values.
                                    Responding with N will cancel agent startup.|
|XPS111A - Enter Recovery Options: RESTART or IGNORE|Indicates the XPS390 LSAM Recovery Management routine has found that scheduled jobs were executing or "in-process" when the system IPL occurred.
                                    Reply "RESTART" to this message if you wish LSAM to report failing jobs to SAM.
                                    Reply, "IGNORE", if you are satisfied that all SAM reconciliation is complete.|
|XPS112A - Two LPARS Cannot Both be LSAM Primary. Enter Sysid of PRIMARY LSAM: [sysid1] or [sysid2]|Indicates more than one LPAR has LSAM=Y coded in the XPSPRMnn membe 
                                    - or -
                                    
                                    Indicates a response to a prior XPS113A message, the operator established a new Primary LSAM Machine ID. Another Primary LSAM is now attempting to enter the OpCon/xps scheduling Sysplex.
                                    Enter the SMF Machine-ID of the LPAR to maintain Primary LSAM Gateway control.
                                    The non-primary LSAM is terminated and you have to restart it after correcting XPSPRMnn parameters.|
|XPS113A - LSAM on [sysid] Has Left the Sysplex. Enter Y/N For This Member to Assume LSAM Status.|Prompts the user if the z/OS LPAR on which this message was issued can adopt all the functions of the Primary LSAM, which has apparently left the Sysplex complex.
                                    A response of "N" returns this LPAR to its normal PSAM status.
                                    A response of "Y" requires you to follow the procedures for LSAM Fail-Over.|
|XPS115E - Security Check Failed RC=nnn-nnn; R15/R0=nnn; Contact Technical Support|Indicates an error occurred when the XPSAFAPI attempted to establish an SAF security environment for XPSAGENT.
                                    Contact SMA Support if the reason for this error cannot be determined.|
|XPS116E - RACDELETE Failed; RC=nnn-nnn; R15/R0=nnn/nnn; Contact Technical Support|Indicates an error occurred when the XPSAFAPI attempted to remove an SAF security environment for XPSAGENT.
                                    Contact SMA Support if the reason for this error cannot be determined.|
|XPS117A - [Userid] Not Defined to Security Subsystem|Indicates an error occurred while the XPSAGENT editor interface was attempting to update JCL in an OpCon/xps defined library.
                                    The [Userid] of the OpCon/xps user attempting the update was not found in RACF, Top Secret, ACF2 or other security system.
                                    Access is denied to this user and the update is not made.
                                    Message XPS122I may precede this message with the name of the JCL member this user was attempting to update.|
|XPS118A - [Userid] Not Authorized to Access [dsname]|Indicates an error occurred while the XPSAGENT editor interface was attempting to update JCL in an OpCon/xps defined library.
                                    The [Userid] of the OpCon/xps user attempting the update was not allowed update authority to this [dsname].
                                    Access is denied to this user and the update is not made.
                                    Message XPS122I may precede this message with the name of the JCL member this user was attempting to update.|
|XPS119E - Tracked/Queued Job: [jobname] Rejected by SAM|Indicates an attempt was made to track a job that was not defined on in SAM.
                                    The job is ignored by OpCon/xps.
                                    All dynamically tracked jobs must have a corresponding entry in SAM.|
|XPS120I - READ|SAVE|EDIT Error|Indicates a requested JCL update has failed.
                                    This message is displayed in a Enterprise Manager Message Box only on the user workstation.
                                    Additional information is available in the MVS SYSLOG.
                                    Retry the request.
                                    If the reason for this error cannot be determined contact SMA support.|
|XPS121E - TCP/IP Error|Indicates a communication error has occurred.
                                    This message is displayed in a Enterprise Manager Message Box only on the user workstation.
                                    Additional information is available in the MVS SYSLOG.
                                    Retry the request.
                                    If the reason for this error cannot be determined, contact SMA support.|
|XPS122I - [Userid] is Updating JCL: [membername] in [ddname]|Indicates OpCon/xps Enterprise Manager Window access updates to production JCL libraries.
                                    This message is displayed on the MVS SYSLOG in an audit trail.|
|XPS123I - OpCon Sysplex Manager Active [versionid]|Indicates Sysplex Management has been initialized.
                                    This message is displayed by XPSPLEX.|
|XPS124W - [qname] Queue is 80% Utilized - Contact SysProg|Indicates that the memory queue [qname] may need to be enlarged using the SETQUES= parameter in the XPSPRMnn library member.
                                    If this message persists or the reason for this message cannot be determined, contact SMA support.|
|XPS126E - Execution Tracking Error: [jobinfo]|Indicates a failure occurred in the SAPI module XPSTATUS while trying to determine the status of a job that appears to have been removed from JES during execution.
                                    If the reason for this error cannot be determined, contact SMA support.|
|XPS127E - Checkpoint Tracking Error: [jobinfo]|Indicates a failure occurred in the SAPI module XPSTATUS while trying to determine the status of a job that appears to have been removed from JES during execution.
                                    If the reason for this error cannot be determined, contact SMA support.|
|XPS128E - IP Recv Error: [errorinfo]|Indicates a failure occurred in the TCP/IP server module XPSERVER.
                                    An IP record was received on the OpCon/xps Port that had no valid header information and had a non-zero length.
                                    If the reason for this error cannot be determined, contact SMA support.|
|XPS196C - Invalid Reply|Indicates an invalid reply was issued in response the XPS110A message.
                                    Only "Y" or "N" is accepted.|
|XPS275I – EXPIRING: [dsnmask|message pattern]|Because it has not been referenced in 45 days, the dataset or message entry is being removed from the monitor table.|
|XPS80xi - Messages|Displays User Exit issues or status.
                                    The x digit refers to the exit number (1 – 9).
                                    The 'I' information character refers to the typical Action/Information Indicator.|
|XPS80xI - Submit Exit [exitname] Active|Indicates the first time a user exit is invoked in normal processing.
                                    The activation of that exit is noted on the console.|
|XPS80xI - User Exit [exitname] Reset|This is a response to an operator command (F LSAM,REPUSERx) or to an internal error in User Exit #n.
                                    The exit has been reset for reload. It is reloaded and refreshed at the next normal exit call.|
|XPS80xE - Invalid Return Code From User Exit|Indicates the exit x has returned a value other than that allowed.
                                    The exit may be deactivated and the current job cancelled.
                                    Valid return codes for each XPS390 user exit is outlined in the XPUSERnn member of hilevel.midlevel.INSTLIB.
                                    Valid return codes are multiples of four starting with zero (e.g., 00, 04, 08, 12…).|
|XPS80xE - Load Failed For XPUSERnn|Indicates the exit referred to by XPUSERnn is not in the //STEPLIB concatenation of the OPCON Proc.
                                    Replace and try again.|
|XPS600I REPLY TO nn IS; [reply]|Indicates the reply referenced (nn) was issued by the LSAM as directed by a WTO Table Event entry.
                                    This message is issued by the OpCon/xps z/OS LSAM Automated Response feature.
                                    This is the OpCon/xps LSAM echo of the IBM IEE600I message. Refer to IEE600I for details.|
|XPS700I REPLY nn IGNORED; REPLY TOO LONG FOR REQUESTOR|Indicates the reply referenced (nn) was attempted by the LSAM as directed by a WTO Table Event entry.
                                    This message is issued by the OpCon/xps z/OS LSAM Automated Response feature.
                                    This is the OpCon/xps LSAM echo of the IBM IEE700I message. Refer to IEE700I for details.|
|XPSI499E - ISPLINK BLDL/FETCH FAILED|Indicates the OpCon/xps ISPF table editor was unable to load the ISPLINK module.|