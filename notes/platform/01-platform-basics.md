# Salesforce Platform Fundamentals

## What is Salesforce?

A cloud-based CRM platform built on a multi-tenant architecture. Every customer
shares the same underlying infrastructure, but data is strictly isolated per org.

## Key Concepts

### Org
A single Salesforce instance/environment. Your company gets one Production org
and can spin up Sandboxes from it.

### Multi-tenancy
All orgs run on shared hardware. Salesforce uses governor limits to prevent any
single org from monopolizing resources (CPU time, SOQL queries, heap size, etc.).
Understanding governor limits is critical — they affect how developers write Apex.

### Metadata
Everything in Salesforce is metadata: objects, fields, page layouts, Apex classes,
flows, profiles, permission sets. This is what gets deployed between environments.

## Org Types

| Type            | Purpose                                  | Data         |
|-----------------|------------------------------------------|--------------|
| Production      | Live environment, real users, real data  | Full         |
| Developer       | Full-featured free org for learning/dev  | None (empty) |
| Sandbox         | Copy of production for dev/test/staging  | Varies       |
| Scratch Org     | Ephemeral, config-driven, SFDX-native    | None         |

### Sandbox Tiers

| Sandbox Type     | Refresh Interval | Data Copied       | Use Case              |
|------------------|------------------|-------------------|-----------------------|
| Developer        | 1 day            | Metadata only     | Individual dev work   |
| Developer Pro    | 1 day            | Metadata only     | Larger dev work       |
| Partial Copy     | 5 days           | Metadata + sample | QA/testing            |
| Full             | 29 days          | Metadata + all    | UAT, perf testing     |

## Governor Limits (Key Ones)

- 100 SOQL queries per transaction
- 150 DML statements per transaction
- 6MB heap size per transaction
- 10s CPU time per transaction

These are per-transaction limits. Bulkification (processing records in collections,
not one at a time) is the standard pattern to stay within them.

## Trailhead

Start here: https://trailhead.salesforce.com/content/learn/modules/starting_force_com
