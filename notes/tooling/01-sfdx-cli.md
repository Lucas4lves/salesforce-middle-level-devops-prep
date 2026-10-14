# SFDX CLI & Tooling

## CLI Versions

Salesforce has been migrating from `sfdx` (legacy) to `sf` (v2 CLI).
Both commands may exist on the same machine. Prefer `sf` for new work.

```bash
# Install
npm install --global @salesforce/cli

# Verify
sf version
sf --help
```

## Project Structure

```
my-sf-project/
├── sfdx-project.json          # Project config (package directories, API version)
├── .forceignore               # Like .gitignore but for Salesforce metadata
├── force-app/
│   └── main/
│       └── default/
│           ├── classes/       # Apex classes (.cls + .cls-meta.xml)
│           ├── triggers/      # Apex triggers
│           ├── objects/       # Custom objects and fields
│           ├── layouts/       # Page layouts
│           ├── flows/         # Flows and process builder
│           └── permissionsets/
└── config/
    └── project-scratch-def.json   # Scratch org definition
```

## Essential Commands

### Org Management
```bash
sf org login web --alias prod                  # OAuth login (browser)
sf org login web --alias dev-sandbox --instance-url https://test.salesforce.com
sf org list                                    # List authenticated orgs
sf org open --target-org my-org               # Open org in browser
sf org display --target-org my-org            # Show org details + access token
```

### Scratch Orgs
```bash
sf org create scratch --definition-file config/project-scratch-def.json --alias my-scratch --duration-days 7
sf org delete scratch --target-org my-scratch
```

### Source / Metadata
```bash
sf project retrieve start --target-org my-org          # Org → local
sf project deploy start --target-org my-org            # Local → org
sf project deploy start --dry-run                      # Validate without deploying
sf project retrieve start --metadata ApexClass         # Retrieve specific type
```

### Apex
```bash
sf apex run --target-org my-org                        # Anonymous Apex REPL
sf apex run test --target-org my-org --test-level RunLocalTests --result-format human
sf apex tail log --target-org my-org                   # Stream debug logs
```

## sfdx-project.json

```json
{
  "packageDirectories": [
    {
      "path": "force-app",
      "default": true
    }
  ],
  "name": "my-project",
  "namespace": "",
  "sourceApiVersion": "59.0"
}
```

## .forceignore

Works like .gitignore. Useful for excluding generated files or metadata
you don't want to track (e.g., standard objects you haven't customized).

```
# Example .forceignore
**/profiles/
**/settings/
```

## VS Code Extension

Salesforce Extension Pack adds:
- Syntax highlighting for Apex, SOQL, LWC
- Org picker in status bar
- Deploy/retrieve on save
- Inline Apex test runner

Install: https://marketplace.visualstudio.com/items?itemName=salesforce.salesforcedx-vscode
