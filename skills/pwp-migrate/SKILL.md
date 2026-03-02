---
name: pwp-migrate
description: "Data and schema migration protocol — safe, reversible, verified migrations. Use this skill whenever the user needs to do a database migration, schema change, data transformation, platform move, or major dependency upgrade. Also use when they say 'add a column', 'change the schema', 'upgrade the database', 'move to a new host', 'backfill data', or 'major version upgrade'. Covers schema, data, platform, and dependency migrations with rollback strategies."
---

# Migration Skill

This skill defines how to plan and execute migrations — database schema changes, data transformations, system upgrades, and platform moves. Migrations are the highest-risk operations in software delivery.

## Migration Mindset

- **Migrations are not features.** They have their own lifecycle and risks. Never mix migration code with application code in the same deploy.
- **Reversibility is mandatory.** Every `up` must have a `down`. If you can't undo it, you're not ready to do it.
- **Test on realistic data.** An empty database is not a test.
- **Communicate risk.** Every migration has a blast radius. Make it explicit.

## Migration Types

| Type | Examples | Risk Level |
|------|---------|-----------|
| **Schema** | Add column, create table, add index | Medium |
| **Data** | Backfill values, transform formats | High |
| **Platform** | Change hosting, switch databases | Very High |
| **Dependency** | Major version upgrade | Medium-High |

## Migration Protocol

### Step 1: Plan
- Document what changes and why
- Identify all affected systems, tables, and services
- Estimate duration, define rollback strategy

### Step 2: Write Migration
Naming: `{YYYYMMDD}_{sequence}_{description}.{direction}.sql`
- Each migration is atomic
- Separate schema changes from data changes
- Never modify an already-applied migration

### Step 3: Test
1. Test on empty database (syntax)
2. Test on seed data (existing data)
3. Test on production-shaped data (realistic volume)
4. Test the rollback
5. Test the application with both old and new schema

### Step 4: Execute
1. Backup the database
2. Deploy to staging first
3. Deploy to production during low-traffic window
4. Monitor for errors
5. Verify data integrity

### Step 5: Verify
- [ ] Migration applied successfully
- [ ] Data integrity checks pass
- [ ] Application functions correctly
- [ ] Rollback tested and verified
- [ ] Performance acceptable

## Safe Schema Change Patterns

**Adding a Column:** Add as nullable → deploy writes → backfill → add constraint
**Removing a Column:** Stop reading → stop writing → drop column
**Renaming a Column:** Add new → write both → backfill → read new → write new → drop old
**Adding an Index:** Use `CREATE INDEX CONCURRENTLY` for large tables

## Data Migration Rules

1. Batch large operations (1,000-10,000 rows per batch)
2. Log progress for long-running migrations
3. Validate before and after (counts, distributions, foreign keys)
4. Never delete data permanently in a migration — soft-delete first

## Anti-Patterns

| Anti-Pattern | Do This Instead |
|-------------|----------------|
| Migration + feature in same deploy | Deploy separately |
| No `down` migration | Always write reversible migrations |
| Testing on empty database only | Test on production-shaped data |
| Editing applied migrations | Write new migration instead |
| No backup before migration | Always backup first |
