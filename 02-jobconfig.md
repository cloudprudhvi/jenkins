# Jenkins Freestyle Job Configuration: Detailed Guide
---

## 1. General

### Description
- **Purpose:** Add a human-readable summary of what the job does.
- **Example:**
  > "This job provisions cloud infrastructure using Terraform scripts."

### Discard Old Builds
- **Purpose:** Automatically delete old build records to save disk space.
- **How to use:**
  - Enable and set the number of builds or days to keep.
- **Example:**
  - Keep only the last 10 builds.

---

## 2. Source Code Management (SCM)

### None
- **Purpose:** No code repository is used (rare for most jobs).

### Git
- **Purpose:** Connects Jenkins to a Git repository to pull code.
- **Key Fields:**
  - **Repository URL:** The Git repo to clone (e.g., `https://github.com/cloudprudhvi/jenkins-terraform.git`).
  - **Credentials:** Used for private repos (leave as "none" for public repos).
  - **Branches to build:** Which branch to use (e.g., `*/master` for the master branch).
- **Example:**
  - URL: `https://github.com/cloudprudhvi/jenkins-terraform.git`
  - Branch: `*/main` or `*/master`

---

## 3. Build Triggers

- **Purpose:** Define when Jenkins should start a build automatically.

### Common Triggers:
- **Build periodically:** Schedule builds using cron syntax (e.g., `H 2 * * *` for daily at 2 AM).
- **Poll SCM:** Check for code changes at intervals (e.g., `H/5 * * * *` for every 5 minutes).
- **GitHub hook trigger for GITScm polling:** Trigger builds when GitHub sends a webhook (recommended for GitHub repos).
- **Trigger builds remotely:** Start builds via an API call (requires an authentication token).

---

## 4. Build Environment

- **Purpose:** Set up the environment before the build starts.

### Common Options:
- **Delete workspace before build starts:** Cleans up files from previous builds.
- **Use secret text(s) or file(s):** Inject secrets (like API keys) securely.
- **Add timestamps to the Console Output:** Adds time info to each log line.
- **Terminate a build if it's stuck:** Automatically kills builds that hang.

---

## 5. Build Steps

- **Purpose:** The main actions Jenkins performs (e.g., compile, test, deploy).

### Execute Shell
- **Purpose:** Run shell commands/scripts.
- **Example:**
  ```sh
  terraform init
  terraform plan
  ```
---

## 6. Post-build Actions

- **Purpose:** Actions after the main build (e.g., notifications, archiving, triggering other jobs).
- **Examples:**
  - Send email notifications on failure.
  - Archive build artifacts (e.g., logs, binaries).
  - Trigger another Jenkins job.

---

## Example: Terraform Job Configuration

- **Description:** "Terraform job to provision infrastructure."
- **SCM:** Git, URL: `https://github.com/cloudprudhvi/jenkins-terraform.git`, Branch: `*/master`
- **Build Triggers:** GitHub webhook
- **Build Environment:** Delete workspace before build, add timestamps
- **Build Steps:**
  ```sh
  terraform init
  terraform plan
  ```
- **Post-build Actions:** Email notification on failure

---
