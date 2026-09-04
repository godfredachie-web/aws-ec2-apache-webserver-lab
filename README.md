# AWS EC2 Web Server Deployment & Network Security Lab

## Executive Summary
This project demonstrates the end-to-end provisioning, system patching, web service configuration, and firewall securing of an Apache web server on an Amazon Web Services (AWS) EC2 Linux instance. The lab highlights fundamental cloud infrastructure and security practices, including compute lifecycle management, least-privilege network access control, remote administration via SSH, package updates, and public web application delivery.

---

## Technical Architecture & Environment Specifications
| Component | Specification / Configuration | Security & Operational Purpose |
| :--- | :--- | :--- |
| **Cloud Provider** | Amazon Web Services (AWS) | Infrastructure-as-a-Service (IaaS) environment |
| **AWS Region** | `eu-north-1` (Stockholm) | Isolated Virtual Private Cloud (VPC) deployment |
| **Compute Instance** | EC2 `t3.micro` | AWS Free Tier general-purpose virtual machine |
| **Operating System** | Amazon Linux 2023 | RPM-based distribution with standard security controls |
| **Web Server** | Apache HTTP Server (`httpd` 2.4) | Production web daemon handling HTTP requests |
| **Public IP Address** | `51.21.201.85` | Publicly routable IPv4 endpoint |
| **Network Security** | AWS Security Group | Stateful host-level ingress filtering (Ports 22 & 80) |

---

## Implementation Phases

### Phase 1: Compute Provisioning & Security Group Hardening
1. Provisioned an EC2 `t3.micro` instance running Amazon Linux 2023 in the `eu-north-1` region.
2. Created and attached a dedicated Security Group defining explicit inbound access controls:
   * **SSH (Port 22):** Ingress permitted for administrative CLI sessions.
   * **HTTP (Port 80):** Ingress permitted from `0.0.0.0/0` to serve public web traffic.

### Phase 2: Remote Connection, OS Patching & Apache Installation
1. Established an encrypted SSH session to the instance using a local RSA private key:
   ```bash
   ssh -i "my-lab-key.pem" ec2-user@51.21.201.85

```

2. Applied system security updates and repository patches:
```bash
sudo dnf update -y

```


3. Installed the Apache HTTP Server package:
```bash
sudo dnf install -y httpd

```


4. Initialized and configured the Apache daemon to persist across system reboots:
```bash
sudo systemctl start httpd
sudo systemctl enable httpd

```


5. Confirmed service execution status (`active (running)`):
```bash
sudo systemctl status httpd

```



### Phase 3: Web Root Administration & Custom Content Authoring

1. Navigated to the standard Apache document root:
```bash
cd /var/www/html

```


2. Authored a custom, responsive `index.html` portfolio file using `nano`:
```bash
sudo nano index.html

```


3. Confirmed code integrity and filesystem output via terminal:
```bash
cat index.html

```



---

## Repository Documentation & Evidence Index

All project artifacts and verification screenshots are structured in the `/screenshots/` directory as follows:

| File Name | Description & Verification Goal |
| --- | --- |
| `01-ec2-instance-running.png` | AWS EC2 Console showing active instance running state and IP `51.21.201.85`. |
| `02-security-group-inbound-rules.png` | Inbound firewall rules showing explicit access for Ports 22 (SSH) and 80 (HTTP). |
| `03-ssh-connection-established.png` | Successful SSH CLI session login to the Amazon Linux 2023 instance. |
| `04-system-update-completed.png` | Complete terminal output verifying clean execution of `sudo dnf update -y`. |
| `05-apache-installation.png` | Package manager output confirming complete installation of `httpd`. |
| `06-apache-status-active.png` | `systemctl` status output confirming `httpd.service` is active and enabled. |
| `07-apache-test-page-live.png` | Web browser rendering default Apache test page at `http://51.21.201.85`. |
| `08-navigate-web-root.png` | Terminal prompt confirming active directory path in `/var/www/html`. |
| `09-nano-editing-index.png` | Active buffer in `nano` text editor displaying custom portfolio HTML structure. |
| `10-nano-save-confirmation.png` | `nano` interface status bar confirming file write destination as `index.html`. |
| `12a-terminal-code.png` | Terminal screenshot verifying source code execution via `cat index.html`. |
| `12b-portfolio-webpage.png` | Full-screen browser verification showing live custom portfolio at `http://51.21.201.85`. |

---

## Key Security & Operational Insights

* **Proactive OS Patching:** Running `sudo dnf update -y` immediately after initial login mitigates baseline Amazon Machine Image (AMI) vulnerabilities prior to exposing public services.
* **Network Attack Surface Reduction:** Separating administration access (Port 22) from standard web presentation access (Port 80) upholds the principle of least privilege.
* **Daemon Resilience:** Enabling services through `systemctl enable` guarantees automatic service recovery following hypervisor disruptions or server reboots.
* **Auditability & Traceability:** Combining CLI file inspection (`cat`) with browser rendering validations ensures complete end-to-end technical documentation.
