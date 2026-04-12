# 🦞 Skill: product-build

> **🇬🇧 English** | [🇨🇳 中文版](../cn/product-build.md)

---

## Metadata

| Field | Value |
|-------|-------|
| Name | `product-build` |
| Version | `1.0.0` |
| Owner | ⚡ Executor |
| Timeout | 1209600s (14 days) |
| Max Retries | 2 |
| Upstream Dependency | `team-assemble` |

---

## Overview

The entrepreneur's ability to "turn ideas into products." Demand is validated, team is assembled — now it's delivery time. OpenClaw coordinates AI coders and human developers from spec to production, fully automated.

---

## Strict Definition

```yaml
skill:
  name: product-build
  version: 1.0.0
  description: "Design, develop, and deliver products that solve validated market needs"
  trigger:
    - event: "team_assembled"
    - event: "sprint_start"
    - event: "iteration_needed"
    - manual: true
  inputs:
    - name: company_id
      type: "string"
      required: true
    - name: validated_requirements
      type: "list[Requirement]"
      required: true
      description: "Validated product requirements list"
    - name: team_roster
      type: "list[TeamMember]"
      required: true
      description: "Team member list"
    - name: sprint_duration_days
      type: "int"
      required: false
      default: 7
      constraints: "1 <= value <= 14"
    - name: mvp_scope
      type: "enum[minimal, standard, full]"
      required: false
      default: "minimal"
      description: "MVP scope: minimal (core), standard (core + supporting), full"
  outputs:
    - name: product_artifacts
      type: "list[Artifact]"
      description: "Product artifacts (code, docs, configs)"
    - name: deployment_url
      type: "string"
      description: "Product access URL"
    - name: test_results
      type: "TestReport"
      description: "Test report"
    - name: user_metrics_baseline
      type: "MetricsBaseline"
      description: "Post-launch initial metrics baseline"
  dependencies:
    skills: ["team-assemble"]
    tools:
      - code_editor
      - test_runner
      - ci_cd_pipeline
      - deployment_manager
      - monitoring_setup
```

---

## Development Lifecycle

```
📋 Spec → 🏗️ Design → 💻 Build → 🧪 Test → 🚀 Deploy → 📈 Measure → 💡 Learn → [Iterate]
```

### Phase 1: Specification 📋

```
Input: validated_requirements (from demand-validate)
Output: ProductSpec document

Auto-executes:
  [ ] Convert user needs to User Stories
  [ ] Define Acceptance Criteria
  [ ] Break into executable Tasks (each <= 4 hours)
  [ ] Estimate effort and prioritize (MoSCoW method)
  [ ] Generate technical specification

  🛡️ Human approval: Confirm scope and priorities
```

### Phase 2: Architecture 🏗️

```
Output: ArchitectureDoc

Auto-executes:
  [ ] Select tech stack (auto-recommend based on project characteristics)
  [ ] Design system architecture diagram
  [ ] Define API contracts (OpenAPI Spec)
  [ ] Design database schema
  [ ] Define security policy (auth, authorization, encryption)
  [ ] Generate Infrastructure as Code (IaC) templates
```

**Tech Stack Recommendation Matrix:**

| Project Type | Frontend | Backend | Database | Deploy |
|-------------|----------|---------|----------|--------|
| Web SaaS | Next.js + Tailwind | FastAPI / Node.js | PostgreSQL + Redis | Vercel + AWS |
| Mobile-first | React Native | Go / Python | PostgreSQL | AWS + CloudFront |
| API Service | - | FastAPI / Go | PostgreSQL + Redis | AWS Lambda |
| Data Platform | React + D3.js | Python | ClickHouse + PG | AWS + dbt |

### Phase 3: Coding 💻

```
Assignment strategy:
  - AI coders: Standard CRUD, API endpoints, unit tests, documentation
  - Human devs: Complex business logic, algorithm optimization, security audit
  - Collaborative: UI implementation, integration tests, performance tuning

Quality gates:
  [ ] Pre-commit auto Lint + Format (ESLint, Prettier, Black)
  [ ] AI Code Review (auto-review every PR)
  [ ] Security scan (dependency vulnerability detection)
  [ ] Code coverage target: > 80%
```

### Phase 4: Testing 🧪

```
Test pyramid:
          ╱   E2E Tests   ╲         ← Few, critical paths
         ╱ Integration Tests╲       ← Moderate, API & module boundaries
        ╱   Unit Tests       ╲      ← Many, core logic
       ╱_____________________╲

Test standards:
  [ ] Unit test coverage > 80%
  [ ] API integration test pass rate 100%
  [ ] E2E tests cover critical user paths
  [ ] Performance: P95 latency < 500ms
  [ ] Security: no high-severity vulnerabilities
  [ ] Compatibility: major browsers/devices
```

### Phase 5: Deployment 🚀

```
Deployment strategy:
  1. Canary release → 5% traffic
  2. Observe 30 minutes, check error rate
  3. Blue-Green deployment → 50% traffic
  4. Observe 2 hours
  5. Full rollout

Deployment checklist:
  [ ] All CI pipelines passing
  [ ] Database migrations executed and rollback-ready
  [ ] Environment variables configured
  [ ] Monitoring and alerting set up
  [ ] Rollback plan tested
```

### Phase 6: Measure & Learn 📈

```
Auto-collect within 7 days post-launch:
  - User registrations and activation rate
  - Core feature usage frequency
  - Error rate and crash logs
  - User feedback (NPS survey)

Auto-generate:
  - Weekly product report
  - Feature usage heatmap
  - Iteration priority recommendations
```

---

## Error Handling

| Error Code | Description | Recovery Strategy |
|------------|-------------|-------------------|
| `BUILD_FAILED` | Build failure | AI analyzes error log, auto-fixes; 3 failures → notify human |
| `TEST_FAILED` | Tests not passing | Block deploy, auto-assign to relevant developer |
| `DEPLOY_FAILED` | Deployment failure | Auto-rollback to last stable version, analyze cause |
| `PERFORMANCE_DEGRADED` | Performance below standard | Auto-scale or optimize code, alert |
| `SECURITY_VULNERABILITY` | Security vulnerability found | Block deploy, flag as urgent fix task |

---

## Monitoring Metrics

```
product_build_cycle_time_seconds                # Development cycle duration
product_build_deploy_frequency                  # Deployment frequency
product_build_test_pass_rate                    # Test pass rate
product_build_bug_escape_rate                   # Bug escape rate
product_build_deployment_success_rate           # Deployment success rate
product_build_mean_time_to_recovery_seconds     # Mean time to recovery
```
