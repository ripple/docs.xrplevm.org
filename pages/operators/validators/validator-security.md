# Validator Security

This document provides a categorized list of recommendations to improve the security of validators in the XRPL EVM sidechain project. Following these guidelines will help ensure a robust and secure environment for your validator nodes.

## Hosting

### Redundant Power

Ensure uninterrupted power supply with redundancy to prevent downtime. This includes the use of uninterruptible power supplies (UPS) for temporary backup and diesel generators or battery arrays for longer power outages. Regularly test and maintain these systems to ensure reliability during emergencies.

### Redundant Networking

Utilize multiple internet service providers (ISPs) or redundant network connections for reliability. This can include setting up failover configurations to automatically switch to an alternative ISP in case of service disruptions. Consider using technologies such as load balancers or software-defined networking (SDN) to optimize and manage network traffic across connections effectively.

### Restricted Physical Access

Limit physical access to the validator hardware to authorized personnel only. This involves setting up security measures such as biometric scanners, access control systems, and surveillance cameras to monitor and control entry points. Additionally, maintain a log of all access attempts and regularly review it for any unauthorized activities.

## General System Security

### Operating System Updates

Keep the operating system patched and up to date. This ensures that known vulnerabilities are addressed promptly, reducing the risk of exploitation by malicious actors. Regularly check for updates from the OS vendor and schedule maintenance windows to apply these patches with minimal disruption.

### Automatic Updates

Configure the operating system to apply security updates automatically. This reduces the administrative burden and ensures critical updates are not missed. Consider implementing a staging environment to test updates before applying them in production to avoid unintended issues.

### Security Framework

Enable and enforce a security framework (e.g., SELinux or AppArmor). These frameworks add an additional layer of security by restricting the actions that applications and users can perform, thereby minimizing the potential impact of compromised processes.

### Minimal Services

Avoid installing unnecessary or insecure services. Review all running services and disable or remove those that are not essential to the validator's operation. This reduces the attack surface and limits potential entry points for attackers.

### GRUB Boot Loader Security

Configure a password for the GRUB boot loader to prevent unauthorized boot changes. This prevents attackers with physical access from modifying boot parameters to bypass security mechanisms. Ensure the password is strong and securely stored.

### File Permissions

Restrict core system files to root permissions only. Use tools like `chmod` and `chown` to ensure sensitive files and directories are accessible only to the root user. Regularly audit file permissions to detect and correct any unintended changes.

### Secure Directories

Ensure the `~/.exrpd` directory is accessible only by the owner. This directory contains sensitive configuration and operational data. Use `chmod 700 ~/.exrpd` to enforce owner-only access and regularly review the directory's contents for unauthorized changes.

## Node Configuration

### Peering Settings

Update `config.toml` with the following recommended settings: - `max_num_inbound_peers = 100` - `max_num_outbound_peers = 10` - `flush_throttle_timeout = "100ms"`

### Disk Optimization

Enable pruning to optimize disk usage. Pruning helps to reduce the storage footprint by removing unnecessary historical data, retaining only the essential information required for current operations. Configure pruning intervals based on the node's storage capacity and performance requirements. Regularly monitor disk usage to ensure the pruning settings are effectively maintaining optimal storage levels without impacting node functionality.

## Remote Access Security

### Password Policies

- **Disallow blank passwords** to prevent unauthorized access and enforce a baseline level of security. Implement mechanisms to check for blank or weak passwords during account creation and regularly audit existing accounts for compliance.
- **Enforce strong password policies**, including minimum length, the use of uppercase and lowercase letters, numbers, and special characters. Require periodic password changes and ensure that old passwords cannot be reused immediately. Additionally, educate users on the importance of secure password practices.

### Multi-Factor Authentication (MFA)

Implement MFA for remote access. Multi-Factor Authentication (MFA) adds an additional layer of security by requiring users to provide two or more verification factors to gain access. This can include a combination of something the user knows (password), something the user has (authentication app or hardware token), and something the user is (biometric data like fingerprints). Ensure that the MFA solution integrates seamlessly with your existing remote access infrastructure and is enforced for all administrative and user accounts.

### Secure SSH Configurations

Secure Shell (SSH) configurations are critical for safeguarding remote access to validator nodes. By enforcing strict SSH policies and best practices, you can significantly reduce the risk of unauthorized access and potential security breaches. Below are the key configurations to implement:

- `PermitRootLogin`: no
- `PasswordAuthentication`: no
- `ChallengeResponseAuthentication`: no
- `UsePAM`: yes
- `AllowUsers`: Specify necessary users only.
- `AllowGroups`: Specify necessary groups only.

### Firewall Configuration

- Enable a firewall to protect all nodes.
- Restrict remote management ports (e.g., SSH - TCP 22) to specific IP addresses.
- Avoid overly permissive rules (e.g., wide range of allowed ports).
- Set specific source and destination addresses for internal node communication.
- For internet-reachable nodes, restrict incoming traffic to TCP 26656 where possible.

## Infrastructure

### Hot Standby Node

Configure a hot standby node with the same settings as the main node. This standby node should remain synchronized with the primary node to ensure it can take over seamlessly in case of failure. Regularly test failover mechanisms to validate that the standby node can function effectively as the primary node when needed. Use automation tools to monitor and manage the synchronization process between the nodes.

### Monitoring and Alerts

Implement a robust system monitoring and alerting framework to notify owners of anomalies. Utilize tools such as Prometheus, Grafana, or equivalent solutions to track system metrics like CPU usage, memory, disk space, and network activity. Set up alerts for critical thresholds and anomalies such as high resource usage, failed processes, or unauthorized access attempts. Ensure the alerting system sends notifications via multiple channels, such as email, SMS, or messaging platforms, to ensure timely response.

### Key Management

Use Horcrux or an equivalent service to replace static key files for secure key management. This approach involves splitting private keys into multiple shares that can be distributed across different secure locations or devices. This minimizes the risk of key compromise. Regularly update and rotate keys as part of your security protocol. Implement strict access controls and logging to monitor key usage and detect any unauthorized activities.

### Sentry Architecture

Follow the sentry architecture setup instructions to improve security and performance. The sentry architecture involves setting up a network of sentry nodes that act as intermediaries between the validator node and external connections. This isolates the validator node from direct exposure to the internet, reducing the risk of DDoS attacks and unauthorized access. Configure sentry nodes with robust security measures, including firewalls, rate limiting, and access controls. Regularly review and update the architecture to address emerging threats.
