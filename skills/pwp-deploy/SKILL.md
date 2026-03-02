---
name: pwp-deploy
description: "Deployment protocol — safe, observable, reversible releases from main to production. Use this skill whenever the user is deploying, releasing, shipping, or pushing to production. Also use when they say 'deploy this', 'ship it', 'push to production', 'release', 'go live', 'pre-deploy check', 'rollback', 'feature flags', or ask about deployment strategies, staging, CI/CD pipelines, or monitoring setup. Covers environment ladder, pre-deploy checklist, deployment strategies, database migration safety, feature flags, rollback plans, and post-deploy verification."
---

# PWP Deployment Protocol

You are now operating under the Project Workflow Protocol's deployment discipline. Follow this structured approach for every deployment task.

## Context: $ARGUMENTS

## Phase 1: Environment Awareness

Understand the environment ladder:
```
local → staging → production
```

| Environment | Purpose | Who Sees It | Data |
|-------------|---------|-------------|------|
| **Local** | Dev and debugging | Developer only | Mock / seed data |
| **Staging** | Pre-production validation | Team / QA | Synthetic data mirroring production |
| **Production** | Real users | Everyone | Real data — handle with care |

### Rules
- Code flows **one direction**: local → staging → production. Never hotfix production directly.
- Staging must mirror production config (same env vars shape, services, region).
- Database migrations run on staging first. Always.

## Phase 2: Pre-Deploy Checklist

Before deploying to production, verify ALL items:

```markdown
- [ ] All pre-merge verification passed (tsc, build, tests)
- [ ] Staging deployment succeeded
- [ ] Staging smoke test passed (critical user journey works)
- [ ] Database migrations tested on staging
- [ ] Environment variables verified (no missing keys)
- [ ] Feature flags configured (if progressive rollout)
- [ ] Rollback plan documented (what to revert and how)
- [ ] Team notified of deploy window
```

## Phase 3: Choose Deployment Strategy

### Direct Deploy (Solo / Small Team)
```
git push origin main → CI builds → auto-deploy → smoke test
```
**When:** Solo builders, early-stage, low-traffic apps.

### Staged Rollout (Growth / Enterprise)
```
main → staging deploy → smoke test → canary (5%) → monitor → full rollout
```
**When:** Significant user base, changes with behavioral risk.

### Blue-Green Deployment (Zero Downtime)
Two identical environments. Switch traffic from old (blue) to new (green).
**When:** Zero-downtime requirement, critical production services.

## Phase 4: Database Migration Safety

Migrations are the highest-risk part of deployment:

1. **Reversible always.** Every `up` has a corresponding `down`.
2. **Two-phase column drops.** Deploy code that stops using the column first. Drop column in next deploy.
3. **Test on production-shaped data.** Not empty databases.
4. **Backup before migration.** Automated or manual — but always before.
5. **Separate migration deploys from feature deploys.** Don't combine schema changes with app logic.

## Phase 5: Feature Flags

For changes with behavioral risk:
```typescript
if (featureFlags.isEnabled('new-checkout-flow')) {
  return <NewCheckout />;
}
return <LegacyCheckout />;
```

### Rules
- Flags are **temporary**. Set a removal date when creating.
- Dead flags (>30 days post-full-rollout) must be cleaned up.
- Flag state tracked in central config, not scattered across code.

## Phase 6: Rollback Strategy

Every deploy must have a documented rollback plan:

| Scenario | Action |
|----------|--------|
| **Code regression** | Revert commit, redeploy previous version |
| **Migration failure** | Run `down` migration, redeploy previous version |
| **Config error** | Fix environment variable, redeploy |
| **Partial outage** | Disable feature flag, investigate |
| **Full outage** | Revert to last known good deploy, incident response |

### Speed Targets
- Code revert: **Under 5 minutes** (automated CI/CD redeploy)
- Migration revert: **Under 15 minutes** (tested `down` migration)
- Full rollback: **Under 30 minutes** (restore from backup if needed)

## Phase 7: Post-Deploy Verification

After every production deploy:
```markdown
- [ ] Application loads without errors
- [ ] Critical user journey works (signup, core action, payment)
- [ ] No new errors in monitoring / error tracking
- [ ] No performance regression (response times, page load)
- [ ] Database is healthy (no locked queries, no pool exhaustion)
- [ ] Logs show expected behavior (no unexpected warnings)
```

## Phase 8: Monitoring & Observability

Set up BEFORE you need it:

| Signal | Tool Examples | What to Watch |
|--------|--------------|---------------|
| **Errors** | Sentry, Bugsnag, LogRocket | New error types, rate spikes |
| **Performance** | Vercel Analytics, Datadog | Response times, Core Web Vitals |
| **Uptime** | UptimeRobot, Pingdom, Better Stack | Downtime, SSL expiry |
| **Logs** | Structured JSON logs | Unexpected patterns, auth failures |
| **Alerts** | PagerDuty, Opsgenie, Slack | Threshold breaches |

### Alert Thresholds
- Error rate > 2x baseline → Slack notification
- Error rate > 5x baseline → Pager alert
- Uptime check fails → Immediate alert
- Response time > 3x baseline → Warning

## Quick Checklist

- [ ] Environment ladder defined
- [ ] Pre-deploy checklist completed
- [ ] Database migrations tested on staging
- [ ] Rollback plan documented
- [ ] Post-deploy verification passed
- [ ] Monitoring active and alerting configured
- [ ] Feature flags have removal dates
