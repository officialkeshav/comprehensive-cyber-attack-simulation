# Comprehensive Cyber Attack Simulation

## Overview

An end-to-end penetration testing and defensive validation project performed in an isolated VMware laboratory environment using Kali Linux and Metasploitable 2.

The project covers the complete security assessment lifecycle from reconnaissance and vulnerability identification to controlled exploitation, post-compromise investigation, remediation, and security re-testing.

## Lab Architecture

| Component | Details |
|---|---|
| Attacker | Kali Linux |
| Attacker IP | 192.168.83.128 |
| Target | Metasploitable 2 |
| Target IP | 192.168.83.129 |
| Network | VMware Host-Only / VMnet1 |

## Objectives

- Perform network reconnaissance
- Enumerate exposed services
- Identify vulnerabilities
- Validate a vulnerability through controlled exploitation
- Verify root-level compromise
- Perform post-compromise discovery
- Identify Indicators of Compromise (IOCs)
- Analyze authentication and service logs
- Apply remediation
- Re-test the affected service

## Attack Lifecycle

Reconnaissance  
↓  
Service Enumeration  
↓  
Vulnerability Assessment  
↓  
Controlled Exploitation  
↓  
Root Access Verification  
↓  
Post-Compromise Discovery  
↓  
IOC / Log Analysis  
↓  
Remediation  
↓  
Security Re-test

## Tools Used

- Kali Linux
- Nmap
- Metasploit Framework
- Meterpreter
- Linux CLI
- VMware Workstation
- Metasploitable 2

## Key Finding

vsFTPd 2.3.4 was identified on TCP/21 and validated as vulnerable to CVE-2011-2523 within the isolated laboratory environment.

## Controlled Exploitation

The vulnerability was validated using the Metasploit Framework module:

`exploit/unix/ftp/vsftpd_234_backdoor`

A Meterpreter session was successfully obtained and privilege verification returned:

`root`

## Post-Compromise Investigation

The compromised system was reviewed for:

- User and home directories
- Web application directories
- Sensitive artifact presence
- SSH configuration
- Authentication-related artifacts
- Suspicious files

No sensitive credential contents are included in this repository.

## Detection / IOC Analysis

The investigation documented:

- Attacker IP: `192.168.83.128`
- Target IP: `192.168.83.129`
- Suspicious artifact: `/AIDxKdZq`
- Artifact SHA-256:
  `ffaa275472a90a45f468f15810a3bf8c3816234695afbfb003b1c85d6ffa1969`
- Successful anonymous FTP authentication
- Multiple FTP connections
- SSH protocol activity
- Telnet, RSH, Rlogin and Rexec activity

The suspicious artifact was not executed or deleted during the investigation.

## Remediation

The vulnerable vsFTPd service was disabled through xinetd.

Original configuration:

`disable = no`

Remediated configuration:

`disable = yes`

The xinetd service was restarted and TCP/21 was verified as no longer listening.

## Validation

### Before Remediation

TCP/21:

`OPEN`

Service:

`vsFTPd 2.3.4`

### After Remediation

TCP/21:

`CLOSED`

The separate ProFTPD service on TCP/2121 remained available and was not modified.

## Repository Structure

```text
comprehensive-cyber-attack-simulation/
│
├── 01-recon/
│   ├── recon-services.txt
│   ├── ftp-enum.txt
│   └── attack-surface-baseline.txt
│
├── 02-vulnerability-assessment/
│   └── vulnerability-scan.txt
│
├── 03-exploitation/
│   └── exploitation-summary.txt
│
├── 04-post-compromise/
│   └── post-compromise-notes.txt
│
├── 05-detection-ioc/
│   ├── ioc-artifact.txt
│   └── ioc-evidence.txt
│
├── 06-remediation/
│   └── remediation.txt
│
└── 07-retest/
    ├── retest-services.txt
    ├── retest-vsftpd.txt
    └── final-verification.txt
