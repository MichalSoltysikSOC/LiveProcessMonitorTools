Tool name: LiveProcessMonitorPlus.exe

Version: 1.0
SHA256 checksum: 10972FBECE7EFB6F349C1B377B39F2E77B5BCA8F511F9CAB1E67181B1926735E
File Size: 84,0 KB
Written in C#, targeting the .NET Framework.
Compiled into a Windows GUI .exe executable file with an MZ file header.

Author: Michał Sołtysik
Cybersecurity Analyst & Consultant | Forensics Examiner | SOC Trainer | Cyber Warfare Organizer
Official website: https://michalsoltysik.com/
LinkedIn: https://www.linkedin.com/in/michal-soltysik-ssh-soc/
Cybersecurity content: https://www.youtube.com/playlist?list=PL0RdRWQWldOAAKBqOVEutxKMP-a6CNoLY
Accredible: https://www.credential.net/profile/michalsoltysik/wallet
Credly: https://www.credly.com/users/michal-soltysik
Email: me@michalsoltysik.com

Purpose: Live Process Monitor Plus extends the baseline-driven monitoring model by correlating native Windows telemetry with optional Sysmon event data, providing deeper timeline visibility while preserving the same workflow and user interface principles.
License: Free for personal and commercial use.

Prerequisite:

(1) Use of Sysmon-based enrichment requires conscious awareness of and compliance with the Sysmon license terms and conditions as published by Microsoft.
(2) To enable Sysmon-based enrichment features in Live Process Monitor Plus, Sysmon must be installed, running, and configured with a compatible configuration file.
(3) The following Sysmon configuration is required to ensure full Live Process Monitor Plus functionality:

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

(4) Alternatively, SysmonConfigurator.exe can be used to configure Sysmon automatically. Its menu provides the following options:

1. Quick Sysmon status check
2. Download and extract Sysmon (C:\Scripts\Sysmon)
3. Create or overwrite Sysmon config (C:\Scripts\SysmonLiveProcessMonitorConfig.xml)
4. Install Sysmon using config (auto-locate Sysmon)
5. Reconfigure Sysmon using config (auto-locate Sysmon)
6. Uninstall Sysmon (auto-locate Sysmon)
7. Sysmon telemetry verification (Event ID 1, 3, 5 - last 15 minutes)
8. Exit

File name: SysmonConfigurator.exe
SHA256 checksum: A5E94D062E3B379E0070B39E695342D7FBA4D5EEF23B834BE3AEF9ACDA9C98DE

Note: SysmonConfigurator.exe is included in the same repository as Live Process Monitor Plus.

Note: SysmonConfigurator.exe relies on the current official Microsoft Sysinternals download location and command-line parameters for Sysmon. If Microsoft changes the download URL (currently https://download.sysinternals.com/files/Sysmon.zip) or modifies Sysmon command-line parameters, the automatic download or configuration functionality may no longer work as expected and will require an update to this tool. In such cases, the issue should be reported to the author of the tool so that the implementation can be reviewed and updated accordingly.

Sysmon-based enrichment (optional):

When Sysmon is installed, running, and properly configured, Live Process Monitor Plus enriches process and network data with additional low-level telemetry:

(1) Correlates Sysmon Event ID 1 (Process creation) with existing process rows.
(2) Correlates Sysmon Event ID 5 (Process terminated) to record precise termination times.
(3) Correlates Sysmon Event ID 3 (Network connection detected) with existing or newly observed network connections.
(4) Displays all observed Sysmon Event IDs (1, 3, 5) per process in a dedicated column.
(5) Adds explicit Sysmon-based timestamps for process creation and termination.

Important behavior:

(1) Sysmon data is inactive during baseline creation.
(2) Sysmon monitoring becomes active only after clicking Resume Monitoring After Baseline Creation.
(3) All Sysmon events occurring before post-baseline monitoring are treated as baseline context.

Note: If Sysmon is not available, the application remains fully functional using native Windows telemetry.

Extended network connection tracking:

Live Process Monitor Plus extends Live Process Monitor with a more complete connection lifecycle view:

(1) Tracks TCP connection start time, last seen time, and end time.
(2) Distinguishes between:

- Active connections.
- Ended connections observed via polling.
- Connection attempts observed only via Sysmon (shown as ATTEMPTED).

(3) Retains ended connections in the session history instead of removing them immediately.
(4) Correlates Sysmon network events with polling-based TCP and UDP snapshots.

Note: Connection colours still follow the owning process classification. Connection state changes are reflected in columns, not via colour transitions.

Summary of Plus-specific capabilities:

Live Process Monitor Plus provides additional analytical depth by:

(1) Combining native Windows process and network telemetry with optional Sysmon event data (Event IDs 1, 3, and 5).
(2) Preserving a detailed timeline of process execution and network activity, including short-lived and rapidly terminating processes.
(3) Using two independent evidence sources - polling-based system telemetry and event-based Sysmon logs - to reliably distinguish baseline context from true post-baseline behavior.

Limitations and operational notes:

(1) Sysmon-based enrichment is optional and conditional. If Sysmon is not installed, not running, or not configured with the required Event IDs, Plus-specific enrichment based on Sysmon data will not be added to the results.
(2) In the absence of Sysmon telemetry, Live Process Monitor Plus operates using native Windows mechanisms only. All core baseline and post-baseline monitoring remains fully functional, but the following Plus-specific capabilities will be unavailable:

- Sysmon-based process creation and termination timestamps.
- Correlation of Sysmon network connection events.
- Identification of connection attempts observed only via Sysmon (ATTEMPTED state).

(3) Sysmon data is collected only during post-baseline monitoring. Events occurring before clicking Resume Monitoring After Baseline Creation are treated as baseline context and are not used for post-baseline correlation.
(4) Sysmon availability and configuration are outside the control of the application. Users are responsible for ensuring that Sysmon is installed, running, and configured with the recommended Event IDs for full Plus functionality.