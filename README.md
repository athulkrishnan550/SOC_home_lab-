<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>SOC Home Lab – Splunk SIEM Security Monitoring Project</title>
  <style>
    :root {
      --bg: #0b1020;
      --panel: #121a2b;
      --panel-2: #18233a;
      --text: #e8eefc;
      --muted: #aab6d3;
      --accent: #4ea1ff;
      --accent-2: #7ef0c5;
      --border: #24314f;
      --code: #0d1424;
      --shadow: 0 10px 30px rgba(0,0,0,.28);
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      font-family: Arial, Helvetica, sans-serif;
      background: linear-gradient(180deg, #08101d 0%, #0f172a 100%);
      color: var(--text);
      line-height: 1.65;
    }

    .container {
      width: min(1100px, 92%);
      margin: 0 auto;
    }

    header {
      padding: 56px 0 28px;
      border-bottom: 1px solid var(--border);
      background: radial-gradient(circle at top right, rgba(78,161,255,.14), transparent 35%),
                  radial-gradient(circle at top left, rgba(126,240,197,.10), transparent 30%);
    }

    .hero {
      display: grid;
      gap: 20px;
    }

    .badge {
      display: inline-block;
      padding: 8px 14px;
      border: 1px solid var(--border);
      border-radius: 999px;
      color: var(--accent-2);
      background: rgba(126,240,197,.08);
      font-size: 14px;
      letter-spacing: .3px;
    }

    h1 {
      margin: 12px 0 8px;
      font-size: clamp(2rem, 4vw, 3.2rem);
      line-height: 1.15;
    }

    .subtitle {
      max-width: 860px;
      color: var(--muted);
      font-size: 1.05rem;
    }

    nav {
      position: sticky;
      top: 0;
      z-index: 50;
      backdrop-filter: blur(10px);
      background: rgba(9, 14, 27, .82);
      border-bottom: 1px solid var(--border);
    }

    nav .container {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      padding: 14px 0;
    }

    nav a {
      color: var(--muted);
      text-decoration: none;
      font-size: 14px;
      padding: 8px 10px;
      border-radius: 10px;
    }

    nav a:hover { color: var(--text); background: rgba(255,255,255,.04); }

    main {
      padding: 34px 0 70px;
    }

    section {
      margin: 26px 0;
      background: linear-gradient(180deg, rgba(255,255,255,.02), rgba(255,255,255,.01));
      border: 1px solid var(--border);
      border-radius: 20px;
      padding: 28px;
      box-shadow: var(--shadow);
    }

    h2 {
      margin-top: 0;
      font-size: 1.7rem;
      color: #ffffff;
    }

    h3 {
      margin-top: 24px;
      font-size: 1.18rem;
      color: var(--accent-2);
    }

    p, li {
      color: var(--text);
    }

    .muted { color: var(--muted); }

    ul, ol {
      padding-left: 22px;
    }

    .grid {
      display: grid;
      gap: 18px;
    }

    .grid-2 {
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    }

    .card {
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 18px;
      padding: 18px;
    }

    .mini-title {
      margin: 0 0 8px;
      font-size: .95rem;
      letter-spacing: .3px;
      color: var(--accent);
      text-transform: uppercase;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      overflow: hidden;
      border-radius: 14px;
      margin-top: 12px;
      background: var(--panel);
    }

    th, td {
      padding: 14px 12px;
      border: 1px solid var(--border);
      text-align: left;
      vertical-align: top;
    }

    th {
      background: var(--panel-2);
      color: #fff;
    }

    code {
      font-family: Consolas, Monaco, monospace;
      background: rgba(255,255,255,.06);
      padding: 2px 6px;
      border-radius: 6px;
      color: #bfe0ff;
    }

    pre {
      background: var(--code);
      color: #d8e6ff;
      padding: 18px;
      border-radius: 16px;
      border: 1px solid var(--border);
      overflow-x: auto;
      box-shadow: inset 0 0 0 1px rgba(255,255,255,.02);
    }

    .architecture {
      white-space: pre;
      font-family: Consolas, Monaco, monospace;
      font-size: 14px;
      line-height: 1.45;
    }

    .placeholder {
      margin-top: 12px;
      padding: 18px;
      border: 1px dashed #42618f;
      border-radius: 16px;
      color: var(--muted);
      background: rgba(78,161,255,.05);
    }

    .footer {
      text-align: center;
      color: var(--muted);
      padding: 26px 0 60px;
      font-size: 14px;
    }

    .tag-row {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-top: 14px;
    }

    .tag {
      padding: 8px 12px;
      border-radius: 999px;
      background: rgba(78,161,255,.09);
      border: 1px solid rgba(78,161,255,.22);
      color: #cbe2ff;
      font-size: 14px;
    }

    .kpi {
      display: grid;
      gap: 6px;
    }

    .kpi strong {
      font-size: 1.5rem;
      color: #fff;
    }

    @media (max-width: 640px) {
      section { padding: 20px; }
      nav .container { gap: 6px; }
      nav a { font-size: 13px; padding: 7px 8px; }
    }
  </style>
</head>
<body>
  <header>
    <div class="container hero">
      <span class="badge">Cybersecurity Portfolio Project</span>
      <div>
        <h1>SOC Home Lab – Splunk SIEM Security Monitoring Project</h1>
        <p class="subtitle">
          A hands-on blue-team project demonstrating SIEM deployment, Windows log forwarding,
          Sysmon telemetry collection, attack simulation, and dashboard-driven security monitoring
          using Splunk Enterprise.
        </p>
      </div>
      <div class="tag-row">
        <span class="tag">Splunk Enterprise</span>
        <span class="tag">Sysmon</span>
        <span class="tag">Windows Event Logs</span>
        <span class="tag">Ubuntu Server</span>
        <span class="tag">Kali Linux</span>
        <span class="tag">SOC Analyst Lab</span>
      </div>
    </div>
  </header>

  <nav>
    <div class="container">
      <a href="#overview">Overview</a>
      <a href="#architecture">Architecture</a>
      <a href="#stack">Tech Stack</a>
      <a href="#implementation">Implementation</a>
      <a href="#dashboard">Dashboard</a>
      <a href="#detections">Detections</a>
      <a href="#screenshots">Screenshots</a>
      <a href="#future">Future Work</a>
    </div>
  </nav>

  <main class="container">
    <section id="overview">
      <h2>Project Overview</h2>
      <p>
        This project simulates a small-scale <strong>Security Operations Center (SOC)</strong> environment using
        <strong>Splunk Enterprise</strong> as the SIEM platform. The lab collects logs from a Windows endpoint,
        forwards them to a centralized Splunk server, and visualizes security-relevant activity through a
        dashboard built in <strong>Splunk Dashboard Studio</strong>.
      </p>
      <p>
        The primary objective is to develop practical, portfolio-ready experience in log ingestion, event analysis,
        threat detection, and security monitoring. The lab also demonstrates how attacker activity, such as an
        <strong>Nmap port scan</strong>, can be captured and analyzed through Sysmon and Windows event logs.
      </p>

      <div class="grid grid-2">
        <div class="card">
          <p class="mini-title">Goal</p>
          <p>Build a realistic SIEM-based home lab that demonstrates core SOC analyst workflows.</p>
        </div>
        <div class="card">
          <p class="mini-title">Index Used</p>
          <p><code>wolfindex</code></p>
        </div>
        <div class="card kpi">
          <p class="mini-title">Environment</p>
          <strong>3 Virtual Machines</strong>
          <span class="muted">Ubuntu, Windows 10, Kali Linux</span>
        </div>
        <div class="card kpi">
          <p class="mini-title">Key Focus</p>
          <strong>Blue-Team Monitoring</strong>
          <span class="muted">Authentication, processes, network activity, applications</span>
        </div>
      </div>
    </section>

    <section id="architecture">
      <h2>Lab Architecture</h2>
      <p>
        The environment contains three systems: a SIEM server, a Windows endpoint, and an attacker machine used for
        controlled simulations.
      </p>
      <pre class="architecture">                 +---------------------+
                 |     Kali Linux      |
                 |     (Attacker)      |
                 +----------+----------+
                            |
                            | Attack Simulation
                            | Nmap / brute force / recon
                            |
                 +----------v----------+
                 |      Windows 10     |
                 |     Victim System   |
                 | Sysmon + Forwarder  |
                 +----------+----------+
                            |
                            | Windows Event Logs
                            | Splunk Universal Forwarder
                            |
                 +----------v----------+
                 |      Ubuntu Server  |
                 |   Splunk Enterprise |
                 |        (SIEM)       |
                 +---------------------+</pre>

      <table>
        <thead>
          <tr>
            <th>Machine</th>
            <th>Role</th>
            <th>Purpose</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Ubuntu Server</td>
            <td>Splunk Enterprise</td>
            <td>Receives, indexes, and visualizes security logs</td>
          </tr>
          <tr>
            <td>Windows 10</td>
            <td>Victim Endpoint</td>
            <td>Generates Windows and Sysmon telemetry</td>
          </tr>
          <tr>
            <td>Kali Linux</td>
            <td>Attacker Machine</td>
            <td>Performs scans and attack simulations for detection testing</td>
          </tr>
        </tbody>
      </table>
    </section>

    <section id="stack">
      <h2>Technologies Used</h2>
      <table>
        <thead>
          <tr>
            <th>Technology</th>
            <th>Purpose</th>
          </tr>
        </thead>
        <tbody>
          <tr><td>Splunk Enterprise</td><td>Central SIEM platform for searching, monitoring, and visualizing logs</td></tr>
          <tr><td>Splunk Universal Forwarder</td><td>Agent installed on Windows to forward logs to Splunk</td></tr>
          <tr><td>Sysmon</td><td>Advanced Windows telemetry for process, network, and system monitoring</td></tr>
          <tr><td>Ubuntu Server</td><td>Host operating system for Splunk Enterprise</td></tr>
          <tr><td>Windows 10</td><td>Log-generating endpoint</td></tr>
          <tr><td>Kali Linux</td><td>Platform for attack simulation and network scanning</td></tr>
          <tr><td>Nmap</td><td>Used to generate network scan events for detection testing</td></tr>
        </tbody>
      </table>
    </section>

    <section id="implementation">
      <h2>Implementation Summary</h2>

      <h3>1. Splunk Installation on Ubuntu</h3>
      <p>Splunk Enterprise was installed on the Ubuntu server and configured as the central SIEM node.</p>
      <pre><code>sudo dpkg -i splunk*.deb
sudo /opt/splunk/bin/splunk start</code></pre>
      <p>Splunk Web was accessed through <code>http://&lt;splunk-server-ip&gt;:8000</code>.</p>

      <h3>2. Enabling Splunk Receiving Port</h3>
      <p>
        The receiving port was configured in Splunk under <code>Settings → Forwarding and Receiving</code>.
        Port <code>9997</code> was enabled to receive forwarded Windows logs.
      </p>

      <h3>3. Windows Log Forwarding Configuration</h3>
      <p>
        The Splunk Universal Forwarder was installed on the Windows machine and configured to send logs to the Splunk server.
        A custom <code>inputs.conf</code> file was created manually in the forwarder directory.
      </p>
      <pre><code>[WinEventLog://Application]
disabled = 0
index = wolfindex

[WinEventLog://Security]
disabled = 0
index = wolfindex

[WinEventLog://System]
disabled = 0
index = wolfindex

[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
index = wolfindex</code></pre>

      <h3>4. Sysmon Deployment</h3>
      <p>
        Sysmon was installed to generate enhanced endpoint telemetry, including process creation and network connection events.
        This was critical for detecting simulated Nmap activity.
      </p>

      <h3>5. Verifying Log Ingestion</h3>
      <p>The following queries were used to validate that log data was arriving correctly in Splunk:</p>
      <pre><code>index=wolfindex
index=wolfindex sourcetype="WinEventLog:Security"
index=wolfindex sourcetype="WinEventLog:Application"
index=wolfindex sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"</code></pre>
    </section>

    <section id="dashboard">
      <h2>Dashboard Design</h2>
      <p>
        A monitoring dashboard was built in <strong>Splunk Dashboard Studio</strong> to visualize event volume,
        user authentication activity, process execution, source IP behavior, and application events.
      </p>

      <table>
        <thead>
          <tr>
            <th>Panel</th>
            <th>Purpose</th>
            <th>Query</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Failed Login Attempts</td>
            <td>Detects brute-force or repeated authentication failures</td>
            <td><code>index=wolfindex EventCode=4625 | stats count by Account_Name</code></td>
          </tr>
          <tr>
            <td>Successful Logins</td>
            <td>Tracks valid authentication activity</td>
            <td><code>index=wolfindex EventCode=4624 | stats count by Account_Name</code></td>
          </tr>
          <tr>
            <td>Process Creation Monitoring</td>
            <td>Identifies frequently executed or suspicious processes</td>
            <td><code>index=wolfindex EventCode=1 | stats count by Image | sort -count</code></td>
          </tr>
          <tr>
            <td>Network Connection Monitoring</td>
            <td>Highlights scan activity and unusual network behavior</td>
            <td><code>index=wolfindex EventCode=3 | stats count by SourceIp DestinationIp DestinationPort</code></td>
          </tr>
          <tr>
            <td>Top Source IP Activity</td>
            <td>Shows the most active event-generating IP addresses</td>
            <td><code>index=wolfindex | stats count by SourceIp | sort -count</code></td>
          </tr>
          <tr>
            <td>Application Event Monitoring</td>
            <td>Tracks software-related events and application log volume</td>
            <td><code>index=wolfindex sourcetype="WinEventLog:Application" | stats count by SourceName | sort -count</code></td>
          </tr>
        </tbody>
      </table>

      <h3>Suggested Dashboard Layout</h3>
      <div class="grid grid-2">
        <div class="card">
          <p class="mini-title">Top Row</p>
          <p>Total Events, Failed Logins, Successful Logins</p>
        </div>
        <div class="card">
          <p class="mini-title">Middle Row</p>
          <p>Top Source IPs, Process Creation Monitoring</p>
        </div>
        <div class="card">
          <p class="mini-title">Bottom Row</p>
          <p>Network Connections, Application Logs</p>
        </div>
      </div>
    </section>

    <section id="detections">
      <h2>Attack Simulation and Detection Use Cases</h2>

      <h3>Nmap Port Scan</h3>
      <p>
        An Nmap scan was launched from the Kali Linux attacker machine against the Windows endpoint. This generated
        network connection telemetry that was captured through Sysmon and forwarded into Splunk.
      </p>
      <pre><code>nmap -sS &lt;windows-ip&gt;</code></pre>
      <p>Relevant detection query:</p>
      <pre><code>index=wolfindex EventCode=3
| stats count by SourceIp DestinationIp DestinationPort</code></pre>

      <h3>Core Security Events Detected</h3>
      <table>
        <thead>
          <tr>
            <th>Event Type</th>
            <th>Detection Logic</th>
            <th>Value to SOC Analysts</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Failed Login Attempts</td>
            <td><code>EventCode=4625</code></td>
            <td>Flags brute-force behavior and repeated account targeting</td>
          </tr>
          <tr>
            <td>Successful Authentication</td>
            <td><code>EventCode=4624</code></td>
            <td>Helps correlate valid logins after failed attempts</td>
          </tr>
          <tr>
            <td>Process Creation</td>
            <td><code>EventCode=1</code></td>
            <td>Tracks newly executed binaries, scripts, or suspicious tools</td>
          </tr>
          <tr>
            <td>Network Connections</td>
            <td><code>EventCode=3</code></td>
            <td>Surfaces scan behavior, beaconing, or unusual connectivity</td>
          </tr>
          <tr>
            <td>Application Events</td>
            <td><code>sourcetype="WinEventLog:Application"</code></td>
            <td>Reveals software faults, crashes, or service anomalies</td>
          </tr>
        </tbody>
      </table>
    </section>

    <section id="screenshots">
      <h2>Screenshots</h2>
      <p>
        Add your project screenshots inside a repository folder such as <code>images/</code>, then update the image paths below.
      </p>

      <div class="placeholder">
        <strong>Suggested screenshot files:</strong><br>
        <code>images/splunk-search-interface.png</code><br>
        <code>images/dashboard-studio-editor.png</code><br>
        <code>images/visualization-options.png</code><br>
        <code>images/dashboard-panel-config.png</code><br>
        <code>images/final-soc-dashboard.png</code>
      </div>

      <h3>Example HTML for images</h3>
      <pre><code>&lt;img src="images/splunk-search-interface.png" alt="Splunk Search Interface" style="width:100%; border-radius:16px; border:1px solid #24314f;"&gt;

&lt;img src="images/final-soc-dashboard.png" alt="Final SOC Dashboard" style="width:100%; border-radius:16px; border:1px solid #24314f; margin-top:16px;"&gt;</code></pre>
    </section>

    <section id="future">
      <h2>Future Improvements</h2>
      <div class="grid grid-2">
        <div class="card">
          <p class="mini-title">Detection Engineering</p>
          <p>Add correlation searches, threshold-based detections, and real-time alerts.</p>
        </div>
        <div class="card">
          <p class="mini-title">MITRE ATT&amp;CK Mapping</p>
          <p>Map each detection use case to relevant ATT&amp;CK techniques for stronger portfolio presentation.</p>
        </div>
        <div class="card">
          <p class="mini-title">Expanded Telemetry</p>
          <p>Include PowerShell logs, Defender logs, firewall logs, and Linux telemetry.</p>
        </div>
        <div class="card">
          <p class="mini-title">Incident Workflow</p>
          <p>Add documented investigations showing triage, analysis, and containment steps.</p>
        </div>
      </div>
    </section>

    <section id="troubleshooting">
      <h2>Common Errors and Troubleshooting During Lab Setup</h2>
      <p>
        During the setup of the SOC home lab several configuration and connectivity issues were encountered.
        Troubleshooting these issues helped develop a deeper understanding of how Splunk forwarding and
        log ingestion works in a SIEM environment.
      </p>

      <h3>1. PowerShell "Unexpected token" Error</h3>
      <p>
        While attempting to run the Splunk CLI command in PowerShell, an error occurred because the path
        contained spaces.
      </p>
      <pre><code>"C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" list forward-server</code></pre>
      <p>
        PowerShell interpreted the command incorrectly. The issue was resolved by using the PowerShell
        call operator <code>&amp;</code>.
      </p>
      <pre><code>&amp; "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" list forward-server</code></pre>

      <h3>2. Forwarder Showing "Configured but Inactive"</h3>
      <p>
        The forward server appeared as configured but not active when running the following command:
      </p>
      <pre><code>splunk list forward-server</code></pre>
      <p>
        This occurred because the receiving port on the Splunk server was not enabled.
      </p>
      <p><strong>Solution:</strong> Enable port <code>9997</code> in Splunk.</p>
      <ol>
        <li>Navigate to <code>Settings → Forwarding and Receiving</code></li>
        <li>Select <code>Configure Receiving</code></li>
        <li>Add new port <code>9997</code></li>
      </ol>

      <h3>3. Missing <code>inputs.conf</code> File</h3>
      <p>
        Initially the Windows Splunk Universal Forwarder directory did not contain an
        <code>inputs.conf</code> file.
      </p>
      <p>
        This file is not created automatically until log inputs are defined. It had to be
        created manually in the following directory:
      </p>
      <pre><code>C:\Program Files\SplunkUniversalForwarder\etc\system\local</code></pre>

      <h3>4. Logs Not Appearing in Splunk Search</h3>
      <p>
        After configuring the forwarder, the Splunk dashboard initially displayed zero events.
        The issue was caused by incorrect search syntax and time range filtering.
      </p>
      <p>
        Running the following query confirmed that logs were successfully arriving:
      </p>
      <pre><code>index=wolfindex</code></pre>

      <h3>5. Incorrect Search Query Syntax</h3>
      <p>
        An early query used the following syntax:
      </p>
      <pre><code>source="WinEventLog:*" index="wolfindex"</code></pre>
      <p>
        This returned no results because Splunk typically filters Windows logs using
        <code>sourcetype</code> rather than <code>source</code>.
      </p>
      <p>Correct query example:</p>
      <pre><code>index=wolfindex sourcetype="WinEventLog:Security"</code></pre>

      <h3>6. Firewall or Network Connectivity Issues</h3>
      <p>
        If the forwarder cannot communicate with the Splunk server, logs will not be sent.
        Connectivity can be tested using:
      </p>
      <pre><code>Test-NetConnection &lt;splunk-ip&gt; -Port 9997</code></pre>

      <p>
        Ensuring that port <code>9997</code> is open on the Splunk server firewall resolves
        this issue.
      </p>

      <p>
        Documenting these issues is valuable because troubleshooting and debugging SIEM
        deployments are important skills for SOC analysts and security engineers.
      </p>
    </section>

    <section>
      <h2>Conclusion</h2>
      <p>
        This project demonstrates a practical SOC monitoring workflow using Splunk, Sysmon, and Windows event logs.
        By building a full pipeline from log generation to dashboard visualization, the lab showcases essential blue-team
        skills such as SIEM deployment, log ingestion, event analysis, and attack detection.
      </p>
      <p>
        It is designed to function as a cybersecurity portfolio project suitable for GitHub, resume linking, and interview discussion.
      </p>
    </section>
  </main>

  <div class="container footer">
    Built as a cybersecurity portfolio project for GitHub showcase.
  </div>
</body>
</html>
