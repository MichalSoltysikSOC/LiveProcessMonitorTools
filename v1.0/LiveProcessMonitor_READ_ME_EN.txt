Tool name: LiveProcessMonitor.exe

Version: 1.0
SHA256 checksum: 37C53E8DC2F940F137A6B9F50855B81F3C37844EF958255FBF660D0742CF8CD1
File Size: 79,0 KB
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

Purpose: Live Process Monitor is a Windows GUI tool designed for baseline-driven endpoint process and network monitoring. It allows the user to create a snapshot baseline of running processes and their network activity, and then transition into continuous post-baseline monitoring to identify new processes, terminated processes, and changes in TCP and UDP network connections. Unlike tools that display only momentary live activity, Live Process Monitor does not discard post-baseline observations over time. All post-baseline process and network activity is continuously collected, retained, and presented as an auditing history, allowing the user to review the full sequence of changes without losing important context. The tool correlates process metadata, command lines, executable hashes, and live network endpoints in a single unified view to support incident response, malware analysis, and live endpoint triage.
License: Free for personal and commercial use.

Design principles:

(1) Baseline-first approach, where all activity observed during baseline creation is treated as trusted reference state.
(2) Post-baseline monitoring that highlights only newly introduced or changed processes while preserving historical visibility.
(3) Session-based evidence retention, where processes and connections remain visible even after termination for investigative review.

Baseline-driven monitoring model:

(1) This tool operates in two distinct phases: baseline creation and post-baseline monitoring.
(2) Baseline data represents the initial known system state and is visually separated from post-baseline activity.
(3) Post-baseline monitoring highlights only newly observed processes and network connections after baseline completion.
(4) This design allows rapid visual identification of suspicious or unexpected activity without prior system knowledge.
(5) All collected data exists only in memory for the current session unless explicitly exported by the user.

What the application does:

(1) Creates a baseline snapshot of running processes using Windows-native mechanisms (WMI and system APIs).
(2) Collects detailed process metadata, including parent-child relationships, executable paths, command lines, and SHA-256 file hashes.
(3) Monitors process lifecycle events in real time, including process start and process termination.
(4) Enumerates active TCP and UDP network endpoints and correlates them with owning processes (PID-based ownership).
(5) Tracks network connection history per process, including first seen time, last seen time, and end time for TCP connections.
(6) Performs optional reverse DNS resolution for public remote IP addresses to provide basic contextual information.

What data is collected for each process:

- PPID (parent process ID)
- PID (process ID)
- Process name (normalized with .exe suffix)
- Full process command line (normalized and expanded when possible)
- Executable file path (when available)
- SHA-256 hash of the executable file on disk (when accessible)
- Process state (Running or No longer running)
- Win32 First Seen timestamp (first observed by the tool)
- Win32 Last Seen timestamp (last observed by the tool)

What data is collected for each network connection:

- TCP connections:

  * Connection state (for example: ESTABLISHED, LISTENING, TIME_WAIT, ENDED)
  * Connection start time (first observed by the tool)
  * Connection end time (set when the connection is no longer observed)
  * Local IP address and source port
  * Remote IP address and destination port
  * Transport protocol (TCP)

- UDP endpoints:

  * Connection state always shown as CONNECTIONLESS
  * Local IP address and source port only
  * Remote IP address and destination port are intentionally not available for UDP
  * Transport protocol (UDP)

Note: UDP is connectionless by design. Remote endpoint information is not available from the operating system and is therefore not displayed.

User interface and displayed data:

The application presents a live-updating table containing two types of rows:

- Process rows (one row per process)
- Connection rows nested directly under the owning process

The table includes the following columns:

- PPID
- PID
- Process Name
- Process Command Line
- Process Path
- Process SHA-256 Hash
- Process State
- Win32 First Seen
- Win32 Last Seen
- Connection State
- Connection Start Time
- Connection End Time
- IP Address
- Reverse DNS
- Source Port
- Destination Port
- Transport Protocol

Note: Connection rows are visually marked with: " +- [connection]"

Colour legend and visual semantics:

The application uses background colours to clearly distinguish baseline and post-baseline activity:

- Light green - baseline process that is still running
- Light blue - baseline process that is no longer running
- Light red - post-baseline process that is still running
- Light pink - post-baseline process that is no longer running
- Light purple - PID 4 (System)

Note: Connection rows do not have independent colour semantics. Each connection row inherits the background colour of its owning process row. As a result, connection rows visually follow the baseline or post-baseline classification of the process they belong to. Connection activity state (for example TCP ENDED) is reflected only in the Connection State and timestamp columns, not via colour changes.

Operational workflow and controls:

(1) Start Monitoring:

- Collects a full baseline snapshot of running processes and their active network connections.
- Marks all observed processes as baseline.

(2) Resume Monitoring After Baseline Creation:

- Starts continuous real-time monitoring.
- New processes and connections observed after this point are classified as post-baseline activity.
- Enables immediate detection of changes relative to the baseline.

(3) Pause Monitoring:

- Temporarily stops all monitoring activity.
- Preserves all collected data.
- Allows inspection or export without losing state.

(4) Resume Monitoring:

- Resumes monitoring from the exact paused state.
- If baseline creation was not fully completed, continues it before entering post-baseline monitoring.

(5) Stop Monitoring:

- Fully stops monitoring.
- Disables all monitoring controls.
- Only Save The Results remains available.

(6) Save The Results:

- Exports the entire table to a CSV file on demand.
- Includes both process rows and nested connection rows.
- Uses UTF-8 encoding and a timestamped filename.
- Defaults to saving on the Desktop.

Core Windows mechanisms used:

(1) Process monitoring:

- Windows Management Instrumentation (WMI) process snapshots to enumerate running processes during baseline creation.
- WMI event tracing (Win32_ProcessStartTrace and Win32_ProcessStopTrace) to detect process start and stop events in real time.
- Native Windows process queries to resolve executable paths and command lines, including fallback mechanisms for short-lived processes.

(2) Network visibility:

- Native Windows IP Helper API to enumerate TCP and UDP endpoints together with owning process identifiers.
- Periodic polling and snapshot correlation to track connection lifecycle, including first seen and end timestamps.
- Process-centric network correlation, presenting network activity as contextual information under each process rather than as a standalone network table.

(3) Executable integrity and attribution:

- Direct file hashing using SHA-256 to uniquely identify process images on disk.
- In-memory hash caching to avoid repeated hashing of identical executables during a session.
- Path resolution fallbacks for system and short-lived utilities to improve attribution accuracy.

(4) Name resolution:

- Reverse DNS lookups for public IP addresses to provide basic destination context.
- In-memory caching of DNS results to avoid repeated lookups during monitoring.

Typical use case:

(1) Start Monitoring to create a baseline of running processes and network activity.
(2) Perform or observe an action of interest on the system (for example executing a sample or installing software).
(3) Click Resume Monitoring After Baseline Creation to track post-baseline changes in real time.
(4) Identify newly started processes, terminated processes, and new or ended network connections using colour cues.
(5) Review command lines, hashes, and network endpoints for investigation.
(6) Save the results to CSV for reporting or further analysis.

Known limitations:

(1) UDP remote endpoints are not available by design and are not displayed.
(2) Reverse DNS resolution is best-effort and may return empty results.
(3) Very short-lived processes may not always yield full path or hash information.
(4) Network monitoring is polling-based; extremely short-lived connections may be missed.