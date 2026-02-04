# 2. Core CI/CD Concepts — Explained

## The Big Picture

```
 You write code
      |
      v
 git push  ──────>  CI PIPELINE RUNS AUTOMATICALLY
                          |
                     +---------+     +---------+     +---------+     +------------+
                     |  LINT   | --> |  TEST   | --> |  BUILD  | --> |   DEPLOY   |
                     +---------+     +---------+     +---------+     +------------+
                     Code style?     Tests pass?     Package it      Ship it!
                     Errors?         Coverage ok?    Create artifact
                          |
                     ANY STAGE FAILS = PIPELINE STOPS = CODE DOES NOT SHIP
```

---

## Concept 1: Continuous Integration (CI)

**What:** Every developer pushes code to a shared repo frequently (at least daily).
Each push automatically triggers a build and test process.

**Why:** Catch bugs early. If you wait weeks to merge, you get massive conflicts
and hidden bugs. CI catches problems within minutes of introducing them.

**Without CI:**
```
Developer A works 2 weeks on feature-A
Developer B works 2 weeks on feature-B
They merge → 200 conflicts, broken tests, 3 days to fix
```

**With CI:**
```
Developer A pushes small changes daily → tests run → pass ✓
Developer B pushes small changes daily → tests run → pass ✓
They merge → 2 conflicts, fixed in 10 minutes
```

**The rule:** If the pipeline is red (failing), STOP and fix it before doing anything else.
A broken pipeline blocks the entire team.

---

## Concept 2: Continuous Delivery (CD)

**What:** Code is always in a deployable state. After CI passes, the code *could*
be deployed to production at any time — but a human makes the final decision.

```
push → lint → test → build → deploy to staging → [HUMAN CLICKS DEPLOY] → production
                                                    ^
                                                    Manual gate
```

**Why:** Reduces risk. You're never more than one click away from a release.
No more "release weekends" where everything breaks.

---

## Concept 3: Continuous Deployment

**What:** Takes Continuous Delivery one step further. Every change that passes
all pipeline stages is deployed to production automatically. No human gate.

```
push → lint → test → build → deploy to staging → deploy to production
                                                   ^
                                                   Fully automatic
```

**Why:** Maximum speed. Used by companies like Netflix, Amazon (thousands of
deployments per day). Requires very high test coverage and monitoring confidence.

---

## CI vs CD vs CD — Summary

| Aspect                 | Continuous Integration | Continuous Delivery | Continuous Deployment |
|------------------------|----------------------|--------------------|-----------------------|
| Auto build & test?     | Yes                  | Yes                | Yes                   |
| Always deployable?     | Not guaranteed       | Yes                | Yes                   |
| Auto deploy to prod?   | No                   | No (manual gate)   | Yes                   |
| Risk level             | Low                  | Low                | Needs strong tests    |
| Speed to production    | Slow (manual)        | Fast (one click)   | Fastest (automatic)   |

---

## Concept 4: Build Automation

**What:** The process of compiling/packaging your code is fully automated
and reproducible. No more "it works on my machine."

**Examples by language:**
```bash
# Python
python -m build                    # Creates .whl and .tar.gz

# JavaScript/TypeScript
npm run build                      # Compiles, bundles, minifies

# Go
go build ./...                     # Compiles binary

# Docker (any language)
docker build -t myapp:v1.2.3 .    # Creates container image

# Java
mvn package                        # Creates .jar
```

**Key principle:** The build must be deterministic. Same code = same output, every time.

---

## Concept 5: Pipeline as Code

**What:** Your CI/CD pipeline is defined in a file (YAML, Groovy, etc.)
that lives in your Git repo alongside your source code.

**Where different tools store it:**
```
GitHub Actions  →  .github/workflows/ci.yml     (this project!)
GitLab CI       →  .gitlab-ci.yml
Jenkins         →  Jenkinsfile
CircleCI        →  .circleci/config.yml
```

**Why this matters:**
- Pipeline changes are reviewed in PRs just like code changes
- You can see the full history of pipeline changes in git log
- New team members see exactly how the project is built and deployed
- No more "click around in a UI to configure the build server"

---

## Concept 6: Artifact Management

**What:** An artifact is any file produced by your pipeline that you want to keep.

```
Source Code  →  [BUILD STAGE]  →  Artifact
                                    |
                                    ├── Docker image (stored in registry)
                                    ├── Python wheel (stored in PyPI/Artifactory)
                                    ├── JS bundle (stored in S3/CDN)
                                    ├── Test report (stored in CI)
                                    └── Coverage report (stored in CI)
```

**Critical rule:** Deploy the SAME artifact that was tested.
Never rebuild for production — that's a different artifact with potentially
different behavior.

```
WRONG:  test(build_1) → deploy(build_2)    # build_2 was never tested!
RIGHT:  test(build_1) → deploy(build_1)    # exact same artifact
```

---

## Concept 7: Environment Promotion

**What:** Code moves through progressively more production-like environments.

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│     DEV      │ --> │   STAGING   │ --> │ PRODUCTION  │
│              │     │             │     │             │
│ - Your local │     │ - Mirrors   │     │ - Real      │
│   machine    │     │   production│     │   users     │
│ - Fast       │     │ - Fake data │     │ - Real data │
│ - Breakable  │     │ - Safe to   │     │ - Must be   │
│              │     │   break     │     │   stable    │
└─────────────┘     └─────────────┘     └─────────────┘
```

**Why:** You'd never test a parachute by jumping out of a plane.
You test it in progressively more realistic conditions first.

---

## Concept 8: Putting It All Together

Look at `.github/workflows/ci.yml` in this project. Here's what happens
when you `git push`:

```
1. TRIGGER:  GitHub detects push to main
                |
2. LINT:     ruff checks code style
                | (pass)
3. TEST:     pytest runs all tests, measures coverage
                | (pass)
4. BUILD:    python -m build creates distributable package
                | (pass)
5. DEPLOY    Artifact deployed to staging environment
   STAGING:     | (pass)
6. DEPLOY    Human approves → deployed to production
   PROD:
```

If ANY stage fails, everything after it is skipped. Your buggy code
never reaches staging or production.

---

## Try It Yourself

After we push this repo to GitHub, try these experiments:

1. **Break a test** — Edit `test_calculator.py`, make a test wrong, push.
   Watch the pipeline fail at the Test stage.

2. **Break the linter** — Add `import os` (unused import) to `calculator.py`, push.
   Watch the pipeline fail at the Lint stage.

3. **Fix and push** — Revert your change, push again. Watch the pipeline go green.

This is the core feedback loop of CI/CD: push → automatic check → fix → push.
