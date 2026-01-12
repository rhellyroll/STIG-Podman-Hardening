# RHEL 9 Podman Hardening Lab  
**Audit-Defensible Container Security on STIG-Aligned Linux Systems**

---

## Executive Summary

This project demonstrates how to securely deploy and operate Podman containers on a hardened RHEL 9 system while maintaining compliance with DISA STIG-aligned security controls.

The lab focuses on enforcing OS-level security boundaries (SELinux, seccomp, systemd, and kernel controls) rather than relying on wrapper scripts or tool defaults. All controls are validated through **direct positive and negative testing**, with audit-grade evidence captured throughout.

**Key Outcome:** Hardened multi-node lab fleet with **rootless Podman**, enforced STIG baselines, **idempotent automation**, and **audit-defensible evidence**.

---

## Environment Overview

| Component | Details |
|---------|--------|
| OS | RHEL 9.x |
| Container Runtime | Podman (rootless where applicable) |
| Nodes | Control, Podman01, Audit01 |
| Security Controls | SELinux (enforcing), seccomp, systemd, firewalld |
| Compliance Alignment | DISA STIG principles |
| Validation Method | OS-level command testing + policy enforcement |

> Scope note: The lab intentionally focuses on **control enforcement and validation**, not production-scale orchestration. Production environments would extend this work using dynamic inventory, CI/CD pipelines, and centralized policy management.

---

## Objectives

- Harden Podman container execution  
- Enforce SELinux container confinement  
- Apply and validate seccomp syscall restrictions  
- Ensure containers operate under systemd governance  
- Maintain compliance-aligned security posture without breaking workloads  
- Produce audit-defensible evidence of enforcement

---

## Security Design Principles

- **Security-first configuration:** OS constrains containers  
- **Least privilege:** Minimal capabilities  
- **Defense in depth:** Kernel, SELinux, seccomp layers  
- **Evidence-driven validation:** Every control tested  
- **Audit realism:** Proof mirrors reviewer expectations

---

## Control Implementation Summary

### SELinux
- Enforcing mode across all nodes  
- Container processes confined to domain-specific policies  
- Denials captured and verified via AVC logs

### Seccomp
- Custom JSON profiles restrict syscalls  
- Unauthorized syscalls blocked with `EPERM`  
- Deny-by-default profile with allowlisting

### systemd
- Containers managed as systemd services  
- Restart policies and lifecycle enforced

### Firewall
- Dedicated container-zone for podman0  
- Drop zone enforces restrictive default-deny posture

### FIPS Pre-Requisites
- Kernel FIPS settings validated  
- System crypto policies verified  
- Ensures Podman workloads comply with enterprise crypto standards

---

## Playbook Execution & Idempotence

### 1️⃣ Full STIG Playbook (~2,000+ Checks)
- Expanded from first project (~700 tasks) to include container-specific hardening, additional nodes, and FIPS pre-requisites  
- **Execution Result:** 2,035 tasks OK, 251 changed  
- **Evidence:** Image 13 – Full playbook summary  
- **Reasoning:** Increase in tasks is due to combining traditional STIG checks with container-specific security automation and multi-node validation

### 2️⃣ Podman Hardening Playbook (Idempotent)
- Focused container security automation  
- Creates seccomp profiles, tests enforcement, configures firewall zones  
- **Execution Result:** 5 OK / 2 changed (expected)  
- **Evidence:** Image 15 – Idempotent run

---

## Validation Evidence (15 Screenshots — reordered to match original lab)

| #  | Screenshot | Validation Purpose |
|----|-----------|-------------------|
| 01 | Audit Node Validation | Confirms audit01 node exists with auditd active |
| 02 | Project Structure Tree | Shows complete Ansible project hierarchy |
| 03 | Rootless Podman Execution | Container runs as non-root ansible user |
| 04 | Seccomp Profile Content | Deny-by-default JSON profile applied |
| 05 | Seccomp Enforcement Test | chmod syscall blocked inside container |
| 06 | System Services Status | firewalld & auditd active, SELinux enforcing |
| 07 | SELinux AVC Denial Logs | Captures blocked login attempts |
| 08 | Firewall Container Zone Creation | container-zone created, podman0 assigned |
| 09 | Firewall Container Zone Config | Interface bindings and settings listed |
| 10 | Firewall Drop Zone | Restrictive default-deny policy displayed |
| 11 | Ansible Inventory Structure | Multi-node architecture validated |
| 12 | Ansible Configuration | roles path, inventory, forks=10, timeout |
| 13 | Full STIG Playbook Execution | 2,035 tasks completed across nodes |
| 14 | Fleet Connectivity Test | Ansible reachability confirmed |
| 15 | Podman Hardening Idempotence | 5 OK / 2 changed pattern, automation proven |

---

## Why This Matters

Many container deployments weaken host security. This lab demonstrates the **opposite approach**:

- Containers operate **within hardened OS constraints**  
- Security controls remain intact  
- Violations fail safely and visibly  
- Evidence supports audit and review requirements  

Applicable to **regulated, compliance-driven Linux environments**.

---

## Key Takeaways

- Secure containerization is an OS problem first, not a tooling problem  
- SELinux and seccomp must be **tested**, not assumed  
- systemd remains authoritative over long-running services  
- Compliance-aligned systems safely support containers  
- Audit evidence is as important as configuration  
- Negative testing validates enforcement

---

## Skills Demonstrated

- RHEL 9 system hardening (DISA STIG)  
- Podman rootless container security  
- SELinux enforcement & AVC log analysis  
- Seccomp syscall restriction testing  
- systemd service management  
- Firewalld zone-based networking  
- Evidence-driven validation  
- Ansible automation with idempotence  
- Multi-node fleet management

---

## Target Environments

- Regulated enterprise & government networks  
- Air-gapped & restricted systems  
- RMF/ATO-aligned platforms  
- FIPS-validated deployments  
- DoD & defense contractor infrastructure

---

## Production Roadmap / Next Lab

- **Next Lab:** Foreman/Satellite containerized deployment  
- Centralized configuration, patching, inventory management  
- Future: CI/CD gates, SCAP scanning, FIPS validation  
> Provides context and planning skills; full production implementation **not required** for this lab

---

## Repository Structure

├── ansible.cfg
├── inventory/
│ └── fleet.ini
├── playbooks/
│ ├── rhel9_stig.yml
│ ├── apply_stig.yml
│ └── podman_hardening.yml
├── roles/
│ ├── rhel9_stig/
│ └── podman_hardening/
├── seccomp/
│ ├── seccomp-restricted1.json
│ └── seccomp-deny-by-default.json
├── docs/
└── evidence/
└── screenshots/ # 15 validation screenshots


---

## Project Classification

**Educational/Portfolio Project**  
Demonstrates practical application of DISA STIGs, CIS Benchmarks, NIST guidelines, and NSA/CISA container hardening guidance in a lab environment.

---

## Additional Notes

- All testing performed in isolated lab environment  
- Evidence screenshots from actual playbook runs  
- Configuration files and playbooks available for review  
- Demonstrates readiness for regulated enterprise environments
