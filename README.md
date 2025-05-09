# day-2-setting-up-wazuh-agent-on-server
setting up the agent on the server and connecting it to the wazuh server
📘 README - Day 2: Setting Up Wazuh Agent on RedHat Server
Date: 2025-05-08
Objective:
Install and configure the Wazuh Agent on a RedHat server and connect it to the Wazuh Manager running on a separate VM (Kali Linux) within the same NAT network.

✅ Steps Performed
Remote Access Setup

Used SSH from Kali Linux to access the RedHat server remotely.

Confirmed both systems were on the same NAT network.

Wazuh Agent Installation (RedHat Server)

Added the Wazuh repository manually since the package was not found via yum initially.

Downloaded and installed the Wazuh agent RPM package.

bash
Copy
Edit
sudo rpm -ivh wazuh-agent-*.rpm
Configuration

Edited the Wazuh Agent configuration file:

bash
Copy
Edit
sudo vi /var/ossec/etc/ossec.conf
Set the <address> tag to point to the Wazuh Manager IP: 10.0.2.6.

Starting Wazuh Agent

Initially failed to start due to a version mismatch between the agent and manager.

Verified logs using:

bash
Copy
Edit
sudo journalctl -xeu wazuh-agent.service
Identified error:

pgsql
Copy
Edit
ERROR: Agent version must be lower or equal to manager version (from manager)
Fix: Upgraded the Wazuh Manager

Resolved version mismatch by upgrading the Wazuh Manager.

Restarted both manager and agent services:

bash
Copy
Edit
sudo systemctl restart wazuh-manager
sudo systemctl restart wazuh-agent
Verification

Confirmed agent connected by checking logs:

bash
Copy
Edit
tail -f /var/ossec/logs/ossec.log
Used telnet from the agent to manager on port 1514 to confirm connectivity:

bash
Copy
Edit
telnet 10.0.2.6 1514
Status Check

Verified agent status:

bash
Copy
Edit
sudo systemctl status wazuh-agent
Confirmed it appeared in the Wazuh Manager's agent list.

⚠️ Troubleshooting Summary
Issue	Fix
Agent service failed to start	Version mismatch error – upgraded Wazuh Manager
Yum could not find package	Used RPM manual installation
Agent not listed in manager	Used correct <address>, restarted services, and verified port connectivity

📝 Notes
Ensure both agent and manager are running compatible Wazuh versions.

After configuration changes, always restart the services.

Use journalctl, ossec.log, and telnet for debugging.

