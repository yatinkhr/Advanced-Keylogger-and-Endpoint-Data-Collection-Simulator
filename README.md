An authorized, lab-only cybersecurity project for studying endpoint telemetry, spyware-like behaviors, and defensive detection engineering.
Overview
This project simulates selected endpoint data-collection behaviors in a controlled environment so that security practitioners can study how such activity appears in process, file, and network telemetry.
The project is designed for ethical security research, detection development, and academic learning. It must not be used to monitor people, collect credentials, access systems without authorization, or transmit private information.
Objectives
•	Understand how input-capture and endpoint collection behaviors generate observable artifacts.
•	Study process, file, and network telemetry associated with suspicious activity.
•	Map observed behaviors to relevant MITRE ATT&CK techniques.
•	Develop behavior-based detection ideas instead of relying only on static signatures.
•	Document defensive controls, investigation steps, limitations, and false-positive considerations.
Safety and Authorization
Use this project only in an isolated lab or on systems that you own or are explicitly authorized to test.
The safe-use requirements are:
•	Use synthetic test data only.
•	Do not collect real passwords, tokens, messages, or personal information.
•	Do not test against another person's device or account.
•	Do not deploy persistence, stealth, evasion, or privilege-escalation features.
•	Do not send collected data to external accounts or third-party services.
•	Prefer a local mock server for testing communication workflows.
•	Stop the program and securely delete test artifacts after experimentation.
The repository is intended to support defensive research. It should not be treated as a ready-to-deploy monitoring or surveillance tool.
Architecture
The project is organized around five conceptual layers:
1.	Simulation — Generates controlled, synthetic endpoint activity.
2.	Collection — Records process, file, and network observations.
3.	Storage — Stores test artifacts locally with appropriate protection.
4.	Detection — Correlates multiple behaviors to identify suspicious activity.
5.	Reporting — Documents timelines, ATT&CK mappings, findings, and mitigations.
+------------------+
| Controlled Lab   |
| Simulation       |
+--------+---------+
         |
         v
+------------------+
| Endpoint         |
| Telemetry        |
+--------+---------+
         |
         v
+------------------+
| Correlation and  |
| Detection        |
+--------+---------+
         |
         v
+------------------+
| Investigation    |
| Report           |
+------------------+

Behaviors Studied
Only document and enable components that are present in the repository and safe for your lab configuration.
•	Synthetic input-capture behavior.
•	Clipboard-access behavior using non-sensitive test values.
•	Screenshot generation for a controlled test desktop.
•	System-information discovery on the lab host.
•	Local encrypted storage of synthetic artifacts.
•	Controlled communication with a local mock service.
•	Process, file, and network event correlation.
The project does not require or encourage collection of real credentials or transmission of user data.
MITRE ATT&CK Mapping
Simulated behavior	Technique	Purpose
Input capture	T1056.001 - Keylogging	Study input-capture telemetry and detection opportunities
Screen capture	T1113 - Screen Capture	Study screenshot-related artifacts
Clipboard access	T1115 - Clipboard Data	Study suspicious clipboard access using synthetic values
Host metadata collection	T1082 - System Information Discovery	Study basic endpoint reconnaissance
Audio collection, if enabled	T1123 - Audio Capture	Study media-access telemetry in a controlled lab
Data transmission, if enabled	Document the exact protocol and purpose	Test controlled communication only

ATT&CK mappings are used for defensive analysis and documentation. They do not authorize deployment of the simulated behavior.
Detection Approach
A single API call or file event should not automatically be treated as malicious because legitimate accessibility, remote-support, and productivity applications may generate similar activity.
A stronger detection hypothesis correlates several signals:
Uncommon or unsigned process
+ Execution from a user-writable directory
+ Repeated creation of log or image artifacts
+ Sensitive input or clipboard access
+ Outbound communication shortly afterward
= Possible unauthorized endpoint data collection

Potential evidence sources include:
•	Process name, path, hash, parent process, and command line.
•	User account and integrity level.
•	File creation and modification events.
•	Network destination, port, and owning process.
•	Digital signature and prevalence of the executable.
•	Persistence-related changes.
•	Timing relationships between collection and network activity.
On Windows, Sysmon or an equivalent endpoint telemetry source can support process, file, and network investigation. On Linux, use appropriate audit, process, filesystem, and network telemetry for the lab environment.
Example Investigation Workflow
1.	Identify the process that generated the suspicious artifact.
2.	Review its parent process, command line, path, hash, and user context.
3.	Establish whether the executable is approved and digitally signed.
4.	Build a timeline of input access, file creation, and network activity.
5.	Check whether synthetic data was staged or transmitted.
6.	Compare the behavior with known legitimate software.
7.	Contain and remove the process if it is unauthorized.
8.	Preserve relevant logs and document the findings.
9.	Improve the detection rule based on false-positive analysis.
Project Structure
Update this section to match the actual repository layout.
.
├── src/                 # Safe simulation and telemetry components
├── detection/           # Detection logic, rules, or queries
├── tests/               # Unit and integration tests
├── docs/                # Architecture, ATT&CK mapping, and findings
├── sample-data/         # Synthetic test data only
├── requirements.txt     # Python dependencies
├── .env.example         # Configuration template without secrets
└── README.md

Requirements
•	Python 3.10 or later.
•	A dedicated virtual machine or isolated lab host.
•	Synthetic test data.
•	Optional endpoint telemetry such as Sysmon, Windows Event Logs, auditd, or an equivalent authorized monitoring source.
•	A local mock service if communication workflows are tested.
Do not commit passwords, API keys, SMTP credentials, tokens, private certificates, screenshots containing personal data, or real endpoint logs.
Installation
Create and activate a virtual environment:
python -m venv .venv
source .venv/bin/activate

On Windows PowerShell:
python -m venv .venv
.\.venv\Scripts\Activate.ps1

Install dependencies:
python -m pip install --upgrade pip
pip install -r requirements.txt

If the repository contains configuration, copy the example file and edit only safe lab values:
cp .env.example .env

Never place secrets in .env.example, source files, notebooks, screenshots, or Git history.
Safe Usage
Before running the project:
1.	Take a snapshot of the lab virtual machine.
2.	Disconnect unnecessary external network access.
3.	Confirm that all test data is synthetic.
4.	Start the local mock service, if required.
5.	Enable the selected endpoint telemetry.
6.	Run the project only for the shortest necessary test period.
7.	Stop all components after the test.
8.	Review and securely delete generated artifacts.
Use the exact commands documented in the repository after verifying that they do not enable persistence, stealth, credential collection, or external transmission.
Testing
Run the test suite with:
pytest -q

Recommended test categories include:
•	Configuration validation.
•	Synthetic-data enforcement.
•	File-encryption and decryption behavior.
•	Error handling when the mock service is unavailable.
•	Cleanup after interrupted execution.
•	Detection-rule matching and non-matching cases.
•	False-positive cases involving approved applications.
Security Design Principles
•	Least privilege: Run with the minimum permissions required for the lab.
•	Explicit consent: Use only authorized systems and synthetic data.
•	No hard-coded secrets: Load test secrets from a secure local configuration mechanism.
•	Local-only testing: Use mock services instead of real external accounts.
•	Clear observability: Prefer visible, auditable execution over stealth.
•	Safe failure: Stop collection and clean up when an error occurs.
•	Data minimization: Collect only the minimum synthetic data required for the experiment.
•	Reproducibility: Record the environment, test case, timestamp, and configuration used.
Limitations
•	This project is not a complete malware-analysis platform or production EDR.
•	Python-level behavior does not represent kernel-level or hardware keyloggers.
•	Telemetry and detection quality depend on the operating system and monitoring configuration.
•	Legitimate accessibility and remote-support tools may create similar events.
•	Results from an isolated lab may not generalize to enterprise environments.
•	The project does not attempt to implement stealth, persistence, evasion, or privilege escalation.
Future Improvements
•	Add a local mock server for all communication tests.
•	Produce structured JSON telemetry with event identifiers.
•	Add Sigma-style detection rules and SIEM queries.
•	Build a timeline dashboard for process, file, and network events.
•	Add automated ATT&CK mapping validation.
•	Measure detection precision, recall, and false-positive rates using synthetic datasets.
•	Add CI checks for secrets, unsafe dependencies, and prohibited functionality.
•	Expand the defensive response playbook for containment and recovery.
Responsible Disclosure and Reporting
If you discover a vulnerability in this repository, do not publish sensitive details immediately. Open a private security report through the repository's configured security contact or GitHub Security Advisories.
Do not submit real credentials, personal information, or data collected from systems that you do not own.
License
Choose and add an appropriate license before publishing. If no license is included, others may not have permission to use, modify, or redistribute the code.
Disclaimer
This repository is provided for authorized cybersecurity education, academic research, and defensive detection development. The author is not responsible for misuse, unauthorized monitoring, privacy violations, data theft, or unlawful activity resulting from this project.

