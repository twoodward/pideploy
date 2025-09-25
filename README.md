# PiDeploy

PiDeploy is a **demo Bash script** that automates the setup of a Linux system for enterprise deployment.  
The script was originally developed to deploy **specialized Raspberry Pis** in an organization for **iPerf network testing**, but was expanded to include additional tools for **security, monitoring, and management**.

⚠️ **Important:** All values in this script are **demo placeholders**. You **must replace them with your organization’s real values** before using in production.

---

## What the Script Does

1. **System Preparation**
   - Updates and upgrades existing packages.
   - Installs common monitoring and diagnostic tools:
     - `iperf3` (network throughput testing)
     - `mtr` (network path diagnostics)
     - `btop` (resource monitoring)
     - `tmux` (terminal multiplexer)

2. **Hostname Setup**
   - Prompts for a new hostname.
   - Updates `/etc/hosts` to avoid resolution issues.

3. **Security Agents**
   - Installs [NinjaOne](https://www.ninjaone.com/) RMM agent (if not already present).
   - Installs [CrowdStrike Falcon](https://www.crowdstrike.com/) sensor and configures it with a CID.

4. **Network Configuration**
   - Configures DNS servers in `/etc/resolv.conf`.
   - Sets up NTP time synchronization via `systemd-timesyncd`.

5. **Active Directory Integration**
   - Installs AD integration packages (`realmd`, `sssd`, `adcli`, etc.).
   - Joins the Linux host to an Active Directory domain.
   - Configures `sssd.conf` for login and group policies.
   - Enables automatic home directory creation.
   - Grants sudo access to a specified AD group.

6. **Monitoring**
   - Installs and configures the Zabbix agent.
   - Sets the agent to start on boot and point to your Zabbix server.

7. **Final Checks**
   - Displays a summary of the system’s configuration when complete.

---

## Placeholders to Update

Before running in your environment, update the following values in the script:

| Placeholder                  | Demo Value                  | Replace With                              |
|------------------------------|-----------------------------|-------------------------------------------|
| `DOMAIN`                     | `demo.local`                | Your AD domain (e.g., `corp.example.com`) |
| `REALM`                      | `DEMO.LOCAL`                | Your uppercase AD realm                   |
| Zabbix server                | `zabbix.demo.local`         | Your Zabbix server FQDN or IP             |
| DNS servers                  | `192.168.1.10, .11, .12`    | Your DNS servers                          |
| NTP server                   | `192.168.1.1`               | Your NTP server                           |
| AD OU                        | `OU=workstations,OU=DEMO...`| Your actual OU path in AD                 |
| AD login group               | `demo_linuxlogin`           | AD group for login access                 |
| AD sudo group                | `demo_linuxsudo`            | AD group for sudo privileges              |
| NinjaOne agent URL           | `https://example.com/ninja-agent.deb` | Your NinjaOne installer link    |
| Falcon sensor URL            | `https://example.com/falcon-sensor.deb` | Your Falcon installer link    |
| Falcon CID                   | `YOUR-FALCON-CID-HERE`      | Your Falcon CID value                     |

---

## Usage

1. Clone this repository:
   ```bash
   git clone https://github.com/your-org/pideploy.git
   cd pideploy
   ```

2. Make the script executable:
   ```bash
   chmod +x deploy.sh
   ```
   This gives the script permission to run.

3. Run the script:
   ```bash
   ./deploy.sh
   ```
   The `./` tells your shell to execute the script from the current directory.

4. Follow prompts (hostname, AD username, etc.).

---

## Notes

- Originally built for **Raspberry Pis performing iPerf testing**, but expanded to deploy extra tools for **security, monitoring, and centralized management**.
- The script is **idempotent**: if a package or service is already installed and running, it skips re-installation.
- Backups are created for modified config files (e.g., `sssd.conf`, `zabbix_agentd.conf`, `/etc/resolv.conf`).
- Requires **sudo** privileges.
- Tested on **Debian 12** (adapt paths as needed for Ubuntu or other distributions).

---

## Disclaimer

This script is provided for **demonstration purposes only**.  
Use at your own risk. Always review and test in a lab environment before deploying to production.
