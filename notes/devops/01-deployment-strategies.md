# Salesforce DevOps & Deployment Strategies

## The Core Problem

Salesforce has no native Git integration. Code and config live inside an org,
not in a file system. The DevOps challenge is keeping orgs in sync with a
source of truth in version control.

## Deployment Approaches

### 1. Change Sets (Legacy)
- GUI-driven, point-and-click
- No version control, no automation, no rollback
- Still common in orgs that haven't modernized
- Good to know for context, not for new projects

### 2. Metadata API
- XML-based deployment via SOAP API
- Foundation of all modern tooling
- Used directly by SFDX under the hood
- `sf project deploy start` wraps this

### 3. SFDX + Source Format (Modern)
- Decomposed metadata (one file per component, not one giant XML blob)
- Works naturally with Git
- Enables CI/CD pipelines
- Recommended approach for all new projects

## Typical Environment Pipeline

```
Developer Scratch Org
        |
        v
   Feature Branch (Git)
        |
        v (PR + CI tests)
   Integration Sandbox
        |
        v (QA sign-off)
     UAT Sandbox
        |
        v (business approval)
      Production
```

## Deployment Tools

| Tool           | Type          | Notes                                      |
|----------------|---------------|--------------------------------------------|
| Salesforce CLI | Native        | sf / sfdx commands, foundation of all else |
| Gearset        | SaaS          | Popular, easy diff/deploy UI               |
| Copado         | Native SF app | Built on Salesforce itself, enterprise     |
| Flosum         | Native SF app | Similar to Copado                          |
| GitHub Actions | DIY CI/CD     | Full control, requires setup               |
| Jenkins        | DIY CI/CD     | Common in enterprise, more ops overhead    |

## Key CLI Commands

```bash
# Authenticate to an org
sf org login web --alias my-org

# Pull metadata from org into local project
sf project retrieve start

# Push local metadata to scratch org
sf project deploy start

# Run Apex tests
sf apex run test --test-level RunLocalTests

# Check deploy status
sf project deploy report
```

## Apex Code Coverage Requirement

Salesforce requires **75% Apex code coverage** to deploy to Production.
This is enforced at deploy time, not optionally. It's a hard gate.

## Key Risks in Large Org Deployments

- Partial deployments that break dependencies
- Test failures blocking deployment (coverage or logic failures)
- Profile/permission set conflicts between orgs
- Managed package version mismatches
- Governor limit violations only appearing at scale
