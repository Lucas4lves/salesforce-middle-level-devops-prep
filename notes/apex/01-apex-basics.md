# Apex Basics (DevOps Perspective)

You don't need to be an Apex developer, but you need to understand what
devs are shipping so you can manage deployments and debug failures.

## What is Apex?

Salesforce's proprietary, Java-like language. Runs server-side on Salesforce
infrastructure, not on your own servers. Strongly typed, object-oriented.

## Syntax Comparison: Java vs Apex

```java
// Java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello");
    }
}
```

```apex
// Apex
public class HelloWorld {
    public static void greet() {
        System.debug('Hello');  // Outputs to debug log, not console
    }
}
```

They're close. Key differences:
- No `main()` method — Apex is invoked by triggers, APIs, or anonymous execution
- `System.debug()` instead of `System.out.println()`
- Built-in SOQL (query language) inline in code
- Governor limits apply to everything

## SOQL — Salesforce Object Query Language

SQL-like, but for Salesforce objects. Used inline in Apex.

```apex
// Get all open opportunities
List<Opportunity> opps = [SELECT Id, Name, Amount FROM Opportunity WHERE StageName != 'Closed Won'];

// Get a single account
Account acc = [SELECT Id, Name FROM Account WHERE Id = :accountId LIMIT 1];
```

Key differences from SQL:
- No `*` — must specify field names
- `FROM` uses Salesforce object API names
- Bind variables use `:variableName` syntax
- JOINs are done via relationship traversal, not JOIN keyword

## Apex Triggers

Run automatically when records are inserted, updated, deleted, etc.

```apex
trigger OpportunityTrigger on Opportunity (before insert, before update) {
    for (Opportunity opp : Trigger.new) {
        if (opp.Amount > 100000) {
            opp.addError('Deals over $100k require VP approval');
        }
    }
}
```

Triggers are the most common source of deployment failures and governor
limit issues in large orgs.

## Test Classes

**Required.** Salesforce won't let you deploy without them.

```apex
@isTest
public class HelloWorldTest {
    @isTest
    static void testGreet() {
        // Setup, execute, assert
        Test.startTest();
        HelloWorld.greet();
        Test.stopTest();
        // No exception = pass
    }
}
```

Rules:
- Test classes must be annotated with `@isTest`
- 75% code coverage required across all Apex to deploy to Production
- Coverage is measured per-line, not per-method
- Test data must be created within the test — tests don't see real org data by default

## Common Deployment Failures You'll Encounter

1. **Test failures** — a test that passed in sandbox fails in prod (usually due to data assumptions)
2. **Coverage below 75%** — new code added without corresponding tests
3. **Missing dependencies** — deploying a class that references an object/field not in the target org
4. **Trigger conflicts** — multiple triggers on the same object firing in unexpected order
5. **SOQL/DML limits** — code that works on small data in sandbox hits limits on prod volume
