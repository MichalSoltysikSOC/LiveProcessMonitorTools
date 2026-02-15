-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Author: Michal Soltysik

Cybersecurity Analyst & Consultant | Forensics Examiner | SOC Trainer | Cyber Warfare Organizer

Official website: https://michalsoltysik.com/

LinkedIn: https://www.linkedin.com/in/michal-soltysik-ssh-soc/

Cybersecurity content: https://www.youtube.com/playlist?list=PL0RdRWQWldOAAKBqOVEutxKMP-a6CNoLY

Accredible: https://www.credential.net/profile/michalsoltysik/wallet

Credly: https://www.credly.com/users/michal-soltysik

Email: me@michalsoltysik.com

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Written in C# and compiled against the .NET Framework 4.x.

All tools are compiled into standalone .exe executable files with a standard MZ file header and run natively on Windows systems.

License: Free for personal and commercial use.

<br>

Repository overview:

Windows GUI tools for baseline-driven endpoint process and network monitoring that capture a snapshot of running processes and connections, then continuously track post-baseline changes without discarding history.
They correlate data in a unified view, with an extended version integrating optional Sysmon telemetry for deeper timeline visibility.

<br>

Tools included:

(1) Live Process Monitor
<br>
(2) Live Process Monitor Plus
<br>
(3) Sysmon Configurator

<br>

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Important Notice - Prerequisite:

<br>

(1) Use of Sysmon-based enrichment requires conscious awareness of and compliance with the Sysmon license terms and conditions as published by Microsoft.

(2) To enable Sysmon-based enrichment features in Live Process Monitor Plus, Sysmon must be installed, running, and configured with a compatible configuration file.

(3) The following Sysmon configuration is required to ensure full Live Process Monitor Plus functionality:

<br>

```xml
<Sysmon schemaversion="4.90">
  <HashAlgorithms>SHA256</HashAlgorithms>
  <EventFiltering>
    <ProcessCreate onmatch="include">
      <Image condition="contains">.exe</Image>
    </ProcessCreate>
    <ProcessTerminate onmatch="include">
      <Image condition="contains">.exe</Image>
    </ProcessTerminate>
    <NetworkConnect onmatch="include">
      <Image condition="contains">.exe</Image>
    </NetworkConnect>
  </EventFiltering>
</Sysmon>
```

<br>

(4) Alternatively, SysmonConfigurator.exe can be used to configure Sysmon automatically. Its menu provides the following options:

<br>

1. Quick Sysmon status check<br>
2. Download and extract Sysmon (C:\Scripts\Sysmon)<br>
3. Create or overwrite Sysmon config (C:\Scripts\SysmonLiveProcessMonitorConfig.xml)<br>
4. Install Sysmon using config (auto-locate Sysmon)<br>
5. Reconfigure Sysmon using config (auto-locate Sysmon)<br>
6. Uninstall Sysmon (auto-locate Sysmon)<br>
7. Sysmon telemetry verification (Event ID 1, 3, 5 - last 15 minutes)<br>
8. Exit<br>

<br>

Note: SysmonConfigurator.exe relies on the current official Microsoft Sysinternals download location and command-line parameters for Sysmon. If Microsoft changes the download URL (currently https://download.sysinternals.com/files/Sysmon.zip) or modifies Sysmon command-line parameters, the automatic download or configuration functionality may no longer work as expected and will require an update to this tool. In such cases, the issue should be reported to the author of the tool so that the implementation can be reviewed and updated accordingly.

<br>

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Summary:

-----------------------------------------
LiveProcessMonitor.exe
-----------------------------------------

Design principles:

<br>

(1) Baseline-first approach, where all activity observed during baseline creation is treated as trusted reference state.

(2) Post-baseline monitoring that highlights only newly introduced or changed processes while preserving historical visibility.

(3) Session-based evidence retention, where processes and connections remain visible even after termination for investigative review.

<br>

Baseline-driven monitoring model:

<br>

(1) This tool operates in two distinct phases: baseline creation and post-baseline monitoring.

(2) Baseline data represents the initial known system state and is visually separated from post-baseline activity.

(3) Post-baseline monitoring highlights only newly observed processes and network connections after baseline completion.

(4) This design allows rapid visual identification of suspicious or unexpected activity without prior system knowledge.

(5) All collected data exists only in memory for the current session unless explicitly exported by the user.

<br>

What the application does:

<br>

(1) Creates a baseline snapshot of running processes using Windows-native mechanisms (WMI and system APIs).

(2) Collects detailed process metadata, including parent-child relationships, executable paths, command lines, and SHA-256 file hashes.

(3) Monitors process lifecycle events in real time, including process start and process termination.

(4) Enumerates active TCP and UDP network endpoints and correlates them with owning processes (PID-based ownership).

(5) Tracks network connection history per process, including first seen time, last seen time, and end time for TCP connections.

(6) Performs optional reverse DNS resolution for public remote IP addresses to provide basic contextual information.

-----------------------------------------
LiveProcessMonitorPlus.exe
-----------------------------------------

When Sysmon is installed, running, and properly configured, Live Process Monitor Plus enriches process and network data with additional low-level telemetry:

<br>

(1) Correlates Sysmon Event ID 1 (Process creation) with existing process rows.

(2) Correlates Sysmon Event ID 5 (Process terminated) to record precise termination times.

(3) Correlates Sysmon Event ID 3 (Network connection detected) with existing or newly observed network connections.

(4) Displays all observed Sysmon Event IDs (1, 3, 5) per process in a dedicated column.

(5) Adds explicit Sysmon-based timestamps for process creation and termination.

-----------------------------------------
SysmonConfigurator.exe
-----------------------------------------

To enable Sysmon-based enrichment features in Live Process Monitor Plus, Sysmon must be installed, running, and configured with a compatible configuration file that enables logging of Sysmon Event ID 1, 3, and 5.

SysmonConfigurator.exe can be used to configure Sysmon automatically.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Intended use:

<br>

This repository is intended for:

<br>

(1) Starting monitoring to create a baseline of running processes and network activity.

(2) Performing or observing an action of interest on the system (for example, executing a sample or installing software).

(3) Clicking Resume Monitoring After Baseline Creation to track post-baseline changes in real time.

(4) Identifying newly started processes, terminated processes, and new or ended network connections using colour cues.

(5) Reviewing command lines, hashes, and network endpoints for investigation.

(6) Saving the results to CSV for reporting or further analysis.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
