## 1️⃣ Poll SCM (Basic Auto Trigger)

**When to use**

* When webhooks are NOT available
* Small projects or labs

**How it works**
Jenkins checks the repo at fixed intervals and triggers a build if changes are detected.

### Example (Declarative Pipeline)

```groovy
pipeline {
    agent any
    triggers {
        pollSCM('H/5 * * * *')  // every 5 minutes
    }
    stages {
        stage('Build') {
            steps {
                echo 'Code changed – Build triggered'
            }
        }
    }
}
```

✅ Simple
❌ Not real-time
❌ Consumes resources

---

## 2️⃣ GitHub Webhook Trigger (Most Recommended)

**When to use**

* Real-time CI
* Production pipelines

### Jenkins Job Configuration

✔️ Check:

```
GitHub hook trigger for GITScm polling
```

### GitHub Webhook

* URL:

```
http://<JENKINS_URL>/github-webhook/
```

* Content type: `application/json`
* Events: `Push`, `Pull Request`

### Pipeline Example

```groovy
pipeline {
    agent any
    triggers {
        githubPush()
    }
    stages {
        stage('Build') {
            steps {
                echo 'Triggered by GitHub webhook'
            }
        }
    }
}
```

✅ Instant build
✅ Best practice

---

## 3️⃣ GitLab Webhook Trigger

### Jenkins Configuration

* Enable **Build when a change is pushed to GitLab**
* Add GitLab webhook in repo

### Pipeline Example

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'GitLab push detected'
            }
        }
    }
}
```

> Trigger is handled externally via webhook

---

## 4️⃣ Cron-Based Scheduled Builds

**When to use**

* Nightly builds
* Security scans
* Reports

### Example

```groovy
pipeline {
    agent any
    triggers {
        cron('H 2 * * *')   // Daily at 2 AM
    }
    stages {
        stage('Nightly Job') {
            steps {
                echo 'Nightly build running'
            }
        }
    }
}
```

📌 Tip: Use `H` to distribute load.

---

## 5️⃣ Trigger After Another Job (Upstream)

**Use case**

* CI → CD chaining

### UI Setting

✔️ Build after other projects are built

```
upstream-job-name
```

### Pipeline Example

```groovy
pipeline {
    agent any
    triggers {
        upstream(upstreamProjects: 'ci-job', threshold: hudson.model.Result.SUCCESS)
    }
    stages {
        stage('Deploy') {
            steps {
                echo 'Triggered after CI success'
            }
        }
    }
}
```

---

## 6️⃣ Trigger from Another Pipeline (Manual Trigger)

### Example

```groovy
build job: 'deploy-job', wait: true
```

Used in **multi-stage CI/CD pipelines**.

---

## 7️⃣ Generic Webhook Trigger (Advanced)

**Use case**

* Trigger from Postman
* Trigger from Python / shell
* Trigger from Terraform / Ansible

### Sample `curl`

```bash
curl -X POST http://jenkins-url/generic-webhook-trigger/invoke \
-H "Content-Type: application/json" \
-d '{"env":"prod"}'
```

### Pipeline

```groovy
pipeline {
    agent any
    stages {
        stage('Triggered') {
            steps {
                echo "Triggered via external system"
            }
        }
    }
}
```

---

## 8️⃣ Multibranch Pipeline Auto Trigger

**Best practice for modern GitOps**

* Auto builds for:

  * Feature branches
  * PRs
  * Main branch

### Jenkinsfile (No trigger needed)

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo "Branch: ${env.BRANCH_NAME}"
            }
        }
    }
}
```

---

## 🔥 Pro Tips (Real-World)

✅ Prefer **webhooks over polling**
✅ Use **multibranch pipelines** for GitHub/GitLab
✅ Combine **cron + webhook** (cron for backup safety)
✅ Secure Jenkins with:

* GitHub App
* Webhook secret
* Token-based triggers

---
