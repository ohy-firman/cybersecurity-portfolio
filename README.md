# Cybersecurity Portfolio

This portfolio highlights my transition from a 4+ year career in data analytics and operational reporting into **Cybersecurity**, covering both **Offensive Security** and **Defensive Operations**. Inside, you will find documented case studies in web application vulnerability assessment, threat detection, SIEM log correlation, and digital forensics investigation.


## About Me

I am a **Cybersecurity Professional** transitioning from a 4+ year career as a Performance Analyst in the financial sector. I recently completed the **Cybersecurity Job Connect Bootcamp at Dibimbing.id**, graduating with **Grade A (92.96/100)** and earning the **MVP Performer Award for Batch 7**.

In my previous role, I monitored daily activity across 2,500+ active funded accounts to detect trading anomalies and operational irregularities, while streamlining Business Intelligence (BI) reporting workflows. This analytical background provides a strong foundation for cybersecurity investigations:

- **Anomaly Detection to Threat Monitoring:** Translating experience in identifying irregular account patterns into analyzing system telemetry, network traffic, and SIEM logs.
- **Risk Profiling to Vulnerability & Incident Analysis:** Applying structured data evaluation to assess technical vulnerabilities (CVSS scoring), trace root causes, and deliver actionable remediation reports.
- **Career Path Preferences:** Penetration Tester, SOC Analyst, InfoSec Analyst, Security Engineer and other Cybersecurity roles.



## Skills & Certifications

### Technical & Security Skills
- **Offensive Security:** Web Application Penetration Testing (OWASP Top 10), Vulnerability Assessment, Privilege Escalation, System Exploitation, CVSS Risk Scoring & Remediation Reporting.
- **Defensive Security:** SIEM Log Analysis & Correlation, Threat Detection & Behavioral Analytics, Network Traffic Analysis, Endpoint Telemetry, Incident Response (NIST Framework), Digital Forensics.
- **Data & Operational Analytics:** Anomaly Detection, Risk Profiling, Log Extraction, Data Visualization, Business Intelligence Reporting.

### Tools Stack
- **Security & Network Tools:** Wazuh SIEM, Burp Suite, Nmap, Wireshark, Triage Sandbox, Cisco Packet Tracer, Kali Linux / Ubuntu CLI.
- **Analytics & Data Tools:** Excel / Google Sheets (Advanced Analytics), Metabase (BI Reporting), MetaTrader 4 (Trade Log Analytics).

### Certifications & Training
- **Cybersecurity Job Connector Bootcamp (Grade A - 92.96/100)** | Dibimbing.id (Nov 2026)
- **MVP Performer Award Cybersecurity Batch 7** | Dibimbing.id (Nov 2026)
- **Registered Financial Planner, RFP®** | FPSB Indonesia (Oct 2026 - Sep 2028)
- **Fast Track - Website Development** | Coding Studio (May 2026)
- **Intro to Data Analytics** | RevoU Indonesia (Feb 2026)



## Project Experience

### Case Study 1: Web Application Penetration Testing (SQLi & Logic Flaw)

#### Introduction
* **Project Title**: Vulnerability Assessment and Penetration Testing (VAPT) on Vuln****
* **Summary/Objective**: Evaluated the authentication security and financial transaction infrastructure of a simulated digital banking application to discover critical input validation flaws before production deployment.
* **Target Information**: `https://vuln****.org` and `https://vuln****.org/login`
* **Tools Used**: Burp Suite, Web Browser Developer Tools.
* **GitHub Link**: [VAPT Findings Report](project-files/Penetration%20Testing%20and%20Findings%20Report.pdf)

#### Reconnaissance & Scanning
* Processed comprehensive endpoint mapping on the main authentication interfaces. Used Burp Suite to intercept and analyze the structure of inbound parameter parameters sent via HTTP POST requests in JSON formats, locating the entry attack surface for exploitation.

#### Exploitation
* **Vulnerability Identified**: 
  1. SQL Injection (Authentication Bypass) on the login page `username` parameter (CVSS v3.1 Score: 9.8 - Critical).
  2. Business Logic Flaw via Transaction Parameter Tampering (CVSS v3.1 Score: 8.1 - High).
* **Exploit Method**: Logic-based query manipulation using an always-true evaluation string and client-side parameter tampering inside raw JSON request streams.
* **Execution Steps**:
  1. Accessed the official digital banking login interface at `https://vuln****.org/login`.
  2. Injected the logic bypass string `lala' or '1'='1'--` directly into the `username` input form while submitting a randomized string on the password field.
  3. Intercepted the outbound payload string using Burp Suite Proxy to verify that the query was delivered directly into the database engine without prior sanitization.
  4. Once authenticated into the dashboard session, navigated to the money transfer facility at the `/transfer` endpoint.
  5. Captured the active transaction request stream using the Burp Suite Repeater module.
  6. Modified the JSON payload data parameters by changing the variable `amount` from a normal positive integer into a negative currency value (`-500`).
  7. Transmitted the modified transaction token stream directly to the application backend server.
* **Proof of Concept (PoC)**: The SQL Injection bypass successfully generated an `HTTP/2 200 OK` validation response along with a valid master session cookie, granting access to a target dashboard account named `bb_a_dj2e318`. For the negative transfer exploit, the server executed the improper mathematical operation on its backend ledger, causing the sender's balance to increase to $1,400.0 and the recipient's balance to lower, while returning a "Transfer Completed" status block.

#### Quick Recap
* **Impact Assessment**: Bypassing primary authentication mechanisms risks client data breaches, while the negative transfer logic compromises the absolute financial integrity of the system ledger.
* **Recommendations**: Enforce parameterized queries (*prepared statements*) across all database search inputs to prevent statement injection. Implement rigid server-side mathematical validation checks ensuring that transaction values are strictly positive integers (`amount > 0`) before ledger commitment.
* **Lessons Learned**: Web applications dealing with financial data must never rely on front-end controls or assume client-side values are safe. Security architectures require complete sanitization of inputs and strict validation of business logic state constraints at the server layer to maintain transactional data integrity.



### Case Study 2: Broken Access Control Vulnerability Assessment

#### Introduction
* **Project Title**: Broken Access Control (BAC) Assessment on OWASP Juice Shop
* **Summary/Objective**: Tested administrative routing, parameter constraints, and token validation architectures within a secure laboratory framework to evaluate authorization mechanisms against real-world exploits.
* **Target Information**: `http://localhost:3000` (OWASP Juice Shop Local Lab via Docker)
* **Tools Used**: Burp Suite, Inspect Element (Browser Developer Tools).
* **GitHub Link**: [Broken Access Control Findings Report](project-files/Broken%20Access%20Control%20Findings%20Report.pdf)

#### Reconnaissance & Scanning
* Analyzed client-side JavaScript behaviors and examined form components using the browser Elements tab. Captured live browser navigation metrics in Burp Suite to analyze state variables and sequential endpoint parameters such as `/rest/basket/`.

#### Exploitation
* **Vulnerability Identified**: 
  1. Weak Administrative Credentials & Forced Browsing (Vertical Privilege Escalation).
  2. Insecure Direct Object Reference / IDOR on View Basket (Horizontal Privilege Escalation).
  3. Client-Side Parameter Tampering on Customer Feedback (Horizontal Privilege Escalation / Impersonation).
* **Exploit Method**: Unauthorized vertical and horizontal privilege escalation via URL forced browsing, ID enumeration, and HTML attribute manipulation.
* **Execution Steps**:
  1. Accessed the target local site inside the browser environment and initiated authentication.
  2. Input the default common system credential keys (`admin@juice-sh.op` / `admin123`) on the primary login modal form to verify default configurations.
  3. Forced the browser URL address path bar to load the unlisted directory route `http://localhost:3000/#/administration` to test vertical privilege controls.
  4. Clicked on the user basket preview tab while running active background traffic interception via Burp Suite Proxy.
  5. Forwarded the logged packet log capture directly to the Repeater section to conduct parameter manipulation.
  6. Edited the destination target endpoint address string, changing the object parameter value sequentially from `/rest/basket/2` to `/rest/basket/1` to attempt horizontal data access.
  7. Accessed the public customer review sub-directory and triggered the Inspect Element framework tab on the web browser.
  8. Found the form field element `<input hidden type="text" id="userId">` inside the HTML structural tree.
  9. Swapped the internal attribute variable to `1` (the master Admin identifier) to check for administrative impersonation, keyed in a mock message text string, and hit the Submit button.
* **Proof of Concept (PoC)**: Validated a severe broken access control across three specific areas. The application loaded the complete list of system users and customer feedback strings via forced browsing without input validation. Changing the destination identity numbers in the backend presented a data leak, returning the operational basket records belonging to other user account. The feedback form accepted the manipulated user variable, officially publishing the comment stream under the Admin identity.

#### Quick Recap
* **Impact Assessment**: Attackers can view private transaction receipts of other users or impersonate corporate executives, severely degrading customer data privacy compliance regulations.
* **Recommendations**: Implement strict server-side authorization enforcement for every sensitive data lookup path. Disregard raw client-controlled ID variables; instead, bind access directly to validation tokens (JWT session attributes) handled on the server backend. Convert all sequential database indexes into non-enumerable, randomized global text identifiers (UUID/GUID).
* **Lessons Learned**: Relying on front-end security through obscurity (such as hiding buttons or relying on hidden form fields) creates a false sense of security. All access right enforcement, session tracking, and parameter authorization must be strictly bound to encrypted token signatures validated exclusively by the backend web controller.



### Case Study 3: Social Engineering & Credential Harvesting Simulation

#### Introduction
* **Project Title**: Credential Harvesting and Web Spoofing Simulation
* **Summary/Objective**: Conducted an automated site replication and credential logging simulation to map data harvesting workflows and evaluate user-side address-bar validation risks.
* **Target Information**: `https://host*****.com`
* **Tools Used**: Social-Engineer Toolkit (SET) on Kali Linux.
* **GitHub Link**: [Social Engineering Findings Report](project-files/Social%20Engineering%20Findings%20Report.pdf)

#### Reconnaissance & Scanning
* Analysed the login form interface and input schemas of the authentication target. The clean, single-card vertical structure allows for seamless, pixel-perfect visual duplication to minimize visual discrepancies during active roleplay simulations.

#### Exploitation
* **Vulnerability Identified**: Susceptibility to lookalike cloning and raw credential POST logging.
* **Exploit Method**: Automated Site Cloning and local IP post-back listener configuration via the SET framework.
* **Execution Steps**:
  1. Opened a root terminal session on the attacker workstation environment and executed the framework using `sudo setoolkit`.
  2. Input option `1` on the console dashboard to access the specialized Social-Engineering Attacks interface.
  3. Entered menu index `2` to pick the core Website Attack Vectors framework layer.
  4. Chosen attack type `3` to activate the standalone Credential Harvester Attack Method workflow.
  5. Selected module index `2` to initialize the automated Site Cloner infrastructure engine.
  6. Provided the local interface address coordinates (`172.16.62.129`) to establish the direct payload return socket path (*POST back* listener).
  7. Keyed in the target target uniform resource locator parameter (`https://host*****.com`) to copy its structure template.
  8. Launched the dynamic background web controller to harvest victim client parameter submissions.
* **Proof of Concept (PoC)**: When a local browser target visited the spoofed attacker host and typed credentials into the login form (`tan*********@gmail.com` / `*******123`), the listener caught the plain-text HTTP POST request packet block, rendering the raw entry variables on the terminal screen and export-saving them as a structured XML session file under `/root/.set/reports/`.

#### Quick Recap
* **Impact Assessment**: Successful harvesting operations yield raw passwords, immediately leading to account takeovers, operational identity fraud, and complete infrastructure boundary breaches.
* **Recommendations**: Multi-Factor Authentication (MFA/2FA) is a mandatory, utilizing dynamic hardware tokens or authenticator apps to minimize stolen passwords risk. Train staff to explicitly verify domain security signatures in the web address bar before authentication. Deploy password managers that limit automated password loading (*auto-fill constraints*) exclusively to matching verified fully qualified domain names.
* **Lessons Learned**: Human-centric vulnerabilities cannot be solved by technical controls alone. Because threat actors can recreate indistinguishable visual environments effortlessly, organizations must integrate structural technological constraints, such as contextual FIDO2 authentication and strict password manager constraints, also enhanced by user awareness training to secure harvesting vectors.


---


Feel free to explore each project! If you have questions or would like more details, don’t hesitate to reach out.

#### Contact Information
**Email**: firman.biz@outlook.com
