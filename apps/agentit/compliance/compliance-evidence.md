# Compliance Evidence Report

**Repository:** AgentIT
**Assessed:** 2026-07-11T13:27:19.226882+00:00
**Overall Score:** 72.4

## Security Controls

### container

- **Status:** partial
- **Severity:** medium
- **Evidence:** No HEALTHCHECK defined in Containerfile
- **Recommendation:** Add HEALTHCHECK for container orchestration readiness probes

### container

- **Status:** partial
- **Severity:** medium
- **Evidence:** Using :latest tag in base image in Containerfile
- **Recommendation:** Pin base image to specific version for reproducible builds

### network

- **Status:** fail
- **Severity:** high
- **Evidence:** No NetworkPolicy manifests found
- **Recommendation:** Add deny-all default NetworkPolicy with explicit allow rules

## Access Controls

### No findings

- **Status:** pass
- **Evidence:** No issues detected in this area.

## Audit Logging

### audit

- **Status:** fail
- **Severity:** high
- **Evidence:** No audit logging implementation detected
- **Recommendation:** Add audit logging for privileged actions and data access

## Data Protection

### No findings

- **Status:** pass
- **Evidence:** No issues detected in this area.
