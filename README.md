# Local Network Port Scan Analysis

**Date:** 2025-08-04
**Intern:** [Mohd Uzair]

## 1. Objective

The objective of this task was to scan the local network (`192.168.31.0/24`) to identify open ports on connected devices and analyze potential security risks.

## 2. Methodology

A TCP Connect scan (`-sT`) was performed using Nmap. The final command used was:
```bash
nmap -sT -oN results.txt -oX results.xml 192.168.31.0/24
```
The full scan output is available in the `results.txt` file in this repository.

### ### Note on Scan Type Selection

The initial task suggested using a TCP SYN scan (`-sS`). However, this scan returned a `dnet: Failed to open device en0` error. This is a known issue on modern macOS, where security features prevent applications from sending the raw packets required for a SYN scan, even when run with root privileges (`sudo`).

To overcome this, the scan type was changed to a TCP Connect scan (`-sT`). This method uses the standard operating system network functions to establish a connection and does not require special permissions, allowing the scan to complete successfully.

## 3. Findings & Risk Analysis

The scan discovered 2 active hosts on the network. The table below summarizes the open ports found and an analysis of the associated risks.

| IP Address      | Hostname            | Open Port     | Service         | Potential Security Risk                                                                                              | Recommendation                                                                          |
|-----------------|---------------------|---------------|-----------------|----------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| `192.168.31.1`  | jiofiber.local.html | 53/tcp        | DNS             | Standard service for a router. No significant risk.                                                                  | None.                                                                                   |
| `192.168.31.1`  | jiofiber.local.html | 80/443/tcp    | HTTP/HTTPS      | Router admin panel. Risk is moderate if the password is weak or default.                                             | Ensure a strong, unique password is set for the router admin page.                      |
| `192.168.31.1`  | jiofiber.local.html | 7443/8080/8443 | Alternate HTTPS | Additional management or proxy ports. Same risk as the primary admin panel.                                          | Ensure services are needed; disable if not. Use strong credentials.                   |
| `192.168.31.208`| Mac.lan             | 5000/tcp      | UPnP            | Allows automatic port forwarding. Could be exploited by malicious software. Risk is low on a trusted network.        | Disable UPnP on the router if not required by specific applications (e.g., for gaming). |
| `192.168.31.208`| Mac.lan             | 7000/tcp      | AirPlay         | Used for screen sharing and media streaming from other Apple devices. Low risk.                                      | Disable AirPlay Receiver in System Settings if the feature is not used.                 |

## 4. Conclusion

The scan revealed a standard home network configuration. The primary device is the router (`192.168.31.1`), which serves DNS and hosts a web administration panel. The second device is the scanning machine itself (`192.168.31.208`). The main area for security hardening would be to ensure the router's administrative password is strong and to evaluate if UPnP functionality is truly necessary. No critical vulnerabilities were discovered.
