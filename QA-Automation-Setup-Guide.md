# QA Automation Setup Guide — Allure, LambdaTest & Grafana

**Project:** `disc-fe-omni-survey-mod` (Survey Modernization)

## Who Should Use This Guide

Use this if you are onboarding onto the Survey Modernization QA automation suite (`e2e/` folder) and need to set up local test execution, cloud browser execution (LambdaTest), test reporting (Allure), and centralized results reporting (Grafana/Postgres).

**Read this alongside the official onboarding page:** [Survey Modernization Onboarding Process](https://adlm.nielseniq.com/confluence/spaces/RBOC/pages/977297771/Survey+Modernization+onboarding+process)

## Setup Summary

| Component | Purpose | Requires Access Request? |
|---|---|---|
| Environment (dev app URL, login) | Run tests against the Survey app | Yes — see Step 1 |
| JFrog (`@gfk/gta` package) | Core GTA framework dependency | Yes — see Step 2 |
| Allure | Local HTML test reports | No |
| LambdaTest | Cloud browser execution | Yes — see Step 3 |
| Cycode | Code security scanning (VS Code plugin) | Yes — see Step 4 |
| Grafana / Postgres | Centralized test-result dashboard | Yes — see Step 5 |

---

## Step 1: Request Environment Access

Before you can run any tests, you need access to the dev environment credentials/URL used by the suite.

Raise a request here: [Environment Access Request Form](https://adlm.nielseniq.com/jira/servicedesk/customer/portal/41/create/986)

Once approved, you'll receive the values needed for `.env`:

```
BASE_URL=<provided dev URL>
USER_NAME=<provided login>
PASS_WORD=<provided password>
```

---

## Step 2: Request JFrog Access (for `@gfk/gta`)

This project depends on the private `@gfk/gta` package, distributed via JFrog Artifactory. If this is your first time on a GTA project, follow the full migration/setup guide here — do not duplicate it, just follow it directly:

[GTA Migration Guide for Existing Users: GitLab to JFrog](https://adlm.nielseniq.com/confluence/pages/viewpage.action?pageId=977291353&spaceKey=DPTD&title=GTA%2BMigration%2BGuide%2Bfor%2BExisting%2BUsers%2BGitLab%2Bto%2BJFrog)

**Quick summary of what that guide covers:**
1. Confirm/request JFrog account access
2. Generate a JFrog read token → set as `JFROG_RO_PASSWORD`
3. Update `.npmrc` at the project root to point at JFrog's registries
4. Clean `node_modules` / `package-lock.json`, reinstall
5. Validate with `npm view @gfk/gta version`

Do this **before** `npm install` in Step 6 below, or `npm install` will fail to resolve `@gfk/gta`.

---

## Step 3: Request LambdaTest Access

LambdaTest is used for cloud browser execution. This project uses a **shared team service account** — do not use a personal LambdaTest login.

1. You should already be added to the LambdaTest group (Survey Mod).
2. Request the shared service-account **username and access key** from **Jeevamurugan S**.
3. Do not share these credentials outside the team or commit them to the repo.

---

## Step 4: Request Cycode Access

Cycode is used for code security scanning (SAST) on this repo, integrated as a VS Code/JetBrains plugin.

1. Raise the access request: [Cycode Access Form](https://adlm.nielsenconnect.com/jiraservicedesk/servicedesk/customer/portal/8/create/403) — use "Please provide the access for Discover frontend" as the request text.
2. Once access is granted, follow the plugin setup steps here: [Cycode - VSCode/JetBrains Plugin (Confluence)](https://adlm.nielseniq.com/confluence/spaces/CSDE/pages/604580440/Cycode+-+VSCode+JetBrains+Plugin)

---

## Step 5: Request Grafana Access

Grafana visualizes test results pushed via the Postgres reporting integration (see Step 9).

1. Raise a request for QA Grafana access via NIQ account, following: **How to Access QA Grafana with NIQ Account** (Data Platform Technology & Data — Confluence)
2. Once granted, confirm you can log into `https://grafana.qa.in.gfk.com/`

---

## Step 6: Clone, Install, Configure

```bash
git clone <repo-url>
cd disc-fe-omni-survey-mod/e2e
npm install
cp .env.example .env
```

Fill in `.env` with:
- Environment values from Step 1
- LambdaTest service-account values from Step 3
- Grafana/Postgres values (already fixed for this repo — see Step 9.1)

All environment variables in this repo use an `SM_` prefix (e.g. `SM_LT_USERNAME`) to avoid colliding with other teams' shared CI variable names.

---

## Step 7: Allure Reporting (No Access Required)

Allure works locally with no credentials. It's the default reporter for every run.

```bash
npm test                 # runs the suite, writes results to allure-results/
npm run allure            # generates and opens the HTML report
```

`npm run allure` runs `allure generate ./allure-results --clean && allure open ./allure-report` under the hood.

**Clearing stale results:**

```bash
rm -rf allure-results/*
npm test
npm run allure
```

---

## Step 8: LambdaTest Cloud Execution

### 8.1 How it's wired

`LAMBDATEST` in `.env` is the default switch (keep it `false` for normal local runs). The dedicated script forces it to `true` regardless of `.env`:

```json
"test:lambdatest": "cross-env LAMBDATEST=true npx playwright test"
```

### 8.2 Required `.env` values

```
LAMBDATEST=false
SM_LAMBDATEST_USERNAME='<service-account-username>'
SM_LT_USERNAME='<service-account-username>'
SM_LAMBDATEST_ACCESS_KEY='<service-account-access-key>'
SM_LT_ACCESS_KEY='<service-account-access-key>'
SM_LT_PROJECT='OMNI SurveyMod'
SM_LT_BUILD='Survey Mod Build'
SM_LT_BUILD_IDENTIFIER=
SM_LT_TUNNEL=true
SM_LT_VIDEO=true
SM_LT_NETWORK=false
SM_LT_CONSOLE=false
SM_LT_OBSERVABILITY=true
SM_LT_IDLE_TIMEOUT=300
SM_LT_WORKERS=1
```

### 8.3 Certificate Setup — "unable to get local issuer certificate"

On a machine behind the corporate proxy (Zscaler), you will very likely hit this on first run:

```
Error: unable to get local issuer certificate
code: 'UNABLE_TO_GET_ISSUER_CERT_LOCALLY'
```

**Cause:** Zscaler re-signs HTTPS traffic with its own certificate, which Node doesn't trust by default.

**Step 8.3.1 — Check if the certificate is already trusted at the OS level:**

```powershell
Get-ChildItem -Path Cert:\LocalMachine\Root | Where-Object { $_.Subject -like "*Zscaler*" }
```

If this returns a result, continue below. If empty, request the certificate from IT.

**Step 8.3.2 — Export the certificate:**

```powershell
$cert = Get-ChildItem -Path Cert:\LocalMachine\Root | Where-Object { $_.Subject -like "*Zscaler*" } | Select-Object -First 1
Export-Certificate -Cert $cert -FilePath "$env:USERPROFILE\zscaler-root-ca.crt" -Type CERT
```

Verify:

```powershell
Test-Path "$env:USERPROFILE\zscaler-root-ca.crt"
```

Should return `True`.

**Step 8.3.3 — Convert to PEM format (Git Bash, has `openssl` built in):**

```bash
openssl x509 -inform DER -in "$USERPROFILE/zscaler-root-ca.crt" -out "$USERPROFILE/zscaler-root-ca.pem"
```

Verify:

```bash
cat "$USERPROFILE/zscaler-root-ca.pem" | head -3
```

Should show `-----BEGIN CERTIFICATE-----`.

**Step 8.3.4 — Point Node at the certificate.** Add to `.env`:

```
NODE_EXTRA_CA_CERTS=C:\Users\<your-username>\zscaler-root-ca.pem
```

`NODE_EXTRA_CA_CERTS` is read by Node at startup, before `.env` loads. If the error persists on first run in a fresh terminal, also export it directly:

```bash
export NODE_EXTRA_CA_CERTS="$USERPROFILE/zscaler-root-ca.pem"
npm run test:lambdatest
```

**Step 8.3.5 — Verify the path is being read correctly:**

```bash
node --env-file=.env -e "console.log(process.env.NODE_EXTRA_CA_CERTS)"
```

Should print the exact path.

### 8.4 Run Against LambdaTest

```bash
npm run test:lambdatest
```

Confirm success by watching for:

```
🌐 LambdaTest Cloud Execution Enabled
   Credentials: configured
Connecting to LambdaTest cloud...
Connected to LambdaTest (Session: Test-<timestamp>)
```

### 8.5 Verify on the LambdaTest Dashboard

Go to Automation → Builds → find the build matching `SM_LT_BUILD` (e.g. "Survey Mod Build") — confirm sessions show pass/fail status and video.

---

## Step 9: Grafana / Postgres Reporting

Pushes test results to a shared Postgres database, visualized on a Grafana dashboard. **Runs only when `CI` is set** — local `npm test` never sends data here, by design.

### 9.1 Registered Team & Project (already done for this repo)

| Level | Value | ID |
|---|---|---|
| Area | FMCG | 1 |
| Domain | DISCOVER | 10 |
| Team | SURVEY MOD | 73 |
| Project | SURVEY-MOD-AUTOMATION | 273 |

`.env.example` values:

```
SM_POSTGRES_API_URL=https://postgres-api.qa.in.gfk.com/report-postgres/api/v1
SM_AREA_NAME=FMCG
SM_DOMAIN_NAME=DISCOVER
SM_TEAM_NAME=SURVEY MOD
SM_PROJECT_NAME=SURVEY-MOD-AUTOMATION
```

If a **new** team/project is ever needed (e.g. a second automation suite under Survey Mod), use Swagger:

```
https://postgres-api.qa.in.gfk.com/report-postgres/api/v1/swagger-ui/index.html
```

1. `GET /areas/find-id-by-name` / `GET /domains/find-id-by-name` / `GET /feature-teams/find-id-by-name` — check what already exists
2. If team missing: `POST /feature-teams/create` with `{ "teamName": "...", "domainId": <id> }`
3. `POST /projects/create` with `{ "projectName": "...", "teamId": <id from step 2> }`
4. Note the returned `projectId`

No authentication required — just being on the NIQ network/VPN.

### 9.2 How It's Wired

`e2e/playwright/reporters/grafana-reporter.ts` — a `GrafanaPlaywrightReporter` class, structurally matching the proven implementation in `disc-fe-omnishopper-ui-automation`.

Registered only in the CI branch of `playwright.config.ts`'s `reporter` array — local runs never include it.

### 9.3 Testing Locally (Forcing CI Mode)

```bash
npx cross-env CI=true npm test
```

> Prefer `cross-env` over typing `CI=true npm test` directly — a shell typo (e.g. `Cl=true` with a lowercase L) silently creates an unused variable with no error, and `CI` stays unset.

### 9.4 Confirming It Worked

Watch for:

```
Custom reporter - Custom options received
Custom reporter - Starting the run with <N> tests
Custom reporter: - Test result created: <resultId>
```

Verify directly via API:

```
GET https://postgres-api.qa.in.gfk.com/report-postgres/api/v1/test-results/projects/273
```

### 9.5 Viewing on the Grafana Dashboard

1. Go to `https://grafana.qa.in.gfk.com/`
2. Search for the shared "Discover" dashboard
3. Set filters: Area=`FMCG`, Domain=`DISCOVER`, Team=`SURVEY MOD`, Project=`SURVEY-MOD-AUTOMATION`
4. Confirm the time range covers your run

**Do not modify the shared dashboard.** Request edit access to create a separate Survey Mod dashboard if needed.

### 9.6 Separate System — "Quest" Portal

Unrelated to Grafana/Postgres above. Quest tracks initiatives/adoption/QMR metrics across engineering teams. Needs its own separate registration if required.

---

## Common Issues and Fixes

**`unable to get local issuer certificate`**
Cause: Zscaler cert not trusted by Node.
Action: Follow Step 8.3 in full.

**LambdaTest session shows wrong username/project**
Cause: `.env` still has personal credentials.
Action: Replace with the shared service-account values from Step 3.

**`401 Unauthorized` from LambdaTest**
Cause: Access key doesn't match username (typo, or wrong value copied).
Action: Re-copy the access key character-by-character from Jeevamurugan S's shared credentials.

**Grafana reporter logs never appear**
Cause: `CI` env var not actually set (often a shell typo).
Action: Use `npx cross-env CI=true npm test`; verify first with `node -e "console.log(process.env.CI)"`.

**"No session mapping found" in LambdaTest teardown logs**
Cause: Stale `allure-results/` from a previous run.
Action: `rm -rf allure-results/*` before a fresh run.

**`Web element with name 'X' not found in object repository`**
Cause: Locator key name mismatch between the page object and the `.locator.json` file.
Action: Check the exact key name in `locators/survey/*.locator.json` against the `getResource()` call.

**403 for `@gfk/*` packages**
Cause: JFrog entitlement issue.
Action: See the JFrog guide's own "Common Issues and Fixes" section (Step 2 link above).

---

## Checklist for New Joiners

- [ ] Environment access requested and approved (Step 1)
- [ ] JFrog access set up, `@gfk/gta` resolves (Step 2)
- [ ] Added to LambdaTest group, service-account credentials obtained from Jeevamurugan S (Step 3)
- [ ] Cycode access requested and plugin configured (Step 4)
- [ ] Grafana access requested and confirmed (Step 5)
- [ ] Repo cloned, `npm install` succeeds, `.env` filled in (Step 6)
- [ ] `npm test` + `npm run allure` produces a local report (Step 7)
- [ ] Zscaler certificate exported and configured (Step 8.3)
- [ ] `npm run test:lambdatest` connects and passes (Step 8.4)
- [ ] `npx cross-env CI=true npm test` shows "Test result created" logs (Step 9.3–9.4)
- [ ] Confirmed own test run visible on Grafana dashboard (Step 9.5)

## Notes for Team Maintainers

- Keep `.env.example` up to date with any new variables — real, safe defaults for non-secret values (Grafana org names), placeholders only for actual credentials.
- Never commit real LambdaTest or JFrog credentials to the repo or CI logs.
- If Team/Project IDs in Postgres ever change (e.g. re-registration), update Step 9.1's table and `.env.example` together.
- Share this page with new joiners as the single starting point — it links out to the official Confluence pages rather than duplicating their content, so update the links here if those pages move.
