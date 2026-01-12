# RHEL 9 Podman Hardening Lab  
**Audit-Defensible Container Security on STIG-Aligned Linux Systems**

---

## Executive Summary

This project demonstrates how to securely deploy and operate Podman containers on a hardened RHEL 9 system while maintaining compliance with DISA STIG-aligned security controls.

The lab focuses on enforcing OS-level security boundaries (SELinux, seccomp, systemd, and kernel controls) rather than relying on wrapper scripts or tool defaults. All controls are validated through direct negative and positive testing, with audit-grade evidence captured throughout.

**In simple terms:** this project shows how to safely add containers to a locked-down Linux system without weakening security or compliance.

---

## Environment Overview

| Component | Details |
|---------|--------|
| OS | RHEL 9.x |
| Container Runtime | Podman (rootless where applicable) |
| Nodes | 3-node lab environment |
| | • Control node |
| | • Podman01 (container host) |
| | • Audit01 (validation node) |
| Security Controls | SELinux (enforcing), seccomp, systemd |
| Compliance Alignment | DISA STIG principles |
| Validation Method | OS-level command testing + policy enforcement |

> Scope note: The lab intentionally focuses on **control enforcement and validation**, not production-scale orchestration. Production environments would extend this work using dynamic inventory, CI/CD pipelines, and centralized policy management.

---

## Objectives

- Harden Podman container execution on RHEL 9
- Enforce SELinux container confinement
- Apply and validate seccomp syscall restrictions
- Ensure containers operate under systemd governance
- Maintain compliance-aligned security posture without breaking workloads
- Produce audit-defensible evidence of enforcement

---

## Security Design Principles

- **Security-first configuration:** Containers adapt to the OS, not the reverse
- **Least privilege:** No unnecessary capabilities or elevated access
- **Defense in depth:** Multiple layers of enforcement (kernel, SELinux, seccomp)
- **Evidence-driven validation:** Every control is tested and verified
- **Audit realism:** Proof mirrors what a security reviewer would expect to see

---

## Control Implementation Summary

### SELinux
- Enforcing mode enabled
- Container processes confined to container-specific domains
- Denials verified using AVC logs and enforcement testing

### Seccomp
- Custom syscall restrictions applied
- Unauthorized syscalls blocked with explicit `EPERM` errors
- Negative testing confirms enforcement (not just configuration)

### systemd Integration
- Containers managed as systemd services
- Restart policies, dependency ordering, and lifecycle control enforced
- No unmanaged or orphaned container processes

### FIPS Alignment
- **FIPS configuration prerequisites enforced**
  - Kernel configuration verified
  - System crypto policies validated
- No unsupported crypto libraries introduced by containers

---

## Validation Methodology

Validation prioritizes **direct OS-level testing** over wrapper scripts or abstracted tooling.

Examples include:
- Verifying SELinux denials via audit logs
- Triggering blocked syscalls to confirm seccomp enforcement
- Confirming container lifecycle behavior through systemd status and logs
- Ensuring containers fail securely when violating policies

This approach ensures controls are **actively enforced**, not merely declared.

---

## Evidence & Screenshots (17 Total)

All screenshots are purpose-selected and audit-relevant.  
No supplemental or decorative images are included.

| # | Screenshot | Validation Purpose |
|--|-----------|-------------------|
| 1 | SELinux enforcing status | Confirms global enforcement |
| 2 | Podman version & runtime info | Establishes execution context |
| 3 | Container SELinux context | Confirms domain confinement |
| 4 | AVC denial log | Evidence of SELinux blocking |
| 5 | seccomp profile applied | Confirms policy attachment |
| 6 | Blocked syscall attempt | Demonstrates enforcement |
| 7 | `EPERM` syscall failure | **Critical negative test** |
| 8 | systemd unit file | Lifecycle governance |
| 9 | `systemctl status` output | Runtime supervision |
|10 | Unauthorized action failure | **Highest-value proof** |
|11 | Audit log correlation | Cross-verification |
|12 | FIPS crypto policy status | Compliance prerequisite |
|13 | Kernel parameter verification | Security baseline |
|14 | Container restart behavior | Resilience control |
|15 | Resource limit enforcement | Stability & safety |
|16 | Clean container shutdown | Orderly lifecycle |
|17 | Final compliance snapshot | End-state verification |

---

## Why This Matters

Many container deployments weaken host security to “make things work.”  
This lab demonstrates the opposite approach:

- Containers operate **within** hardened constraints
- Security controls remain intact
- Violations fail safely and visibly
- Evidence supports audit and review requirements

This is directly applicable to regulated, compliance-driven Linux environments.

---

## Key Takeaways

- Secure containerization is an OS problem first, not a tooling problem
- SELinux and seccomp must be *tested*, not trusted
- systemd remains the authority over long-running services
- Compliance-aligned systems can support containers without exceptions
- Audit evidence is as important as configuration

---

## Skills Demonstrated

- RHEL 9 system hardening
- Podman container security
- SELinux policy enforcement and validation
- seccomp syscall restriction testing
- systemd service management
- Compliance-aligned engineering judgment
- Security-focused troubleshooting and analysis

---

## Disclaimer

This project is a controlled lab environment designed to demonstrate security concepts and enforcement techniques. Production deployments may require additional scaling, monitoring, and automation based on organizational requirements.

---

## Author
  
Linux Systems Administration | Platform Security | Automation
