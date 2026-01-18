## DRY Concept (Don’t Repeat Yourself)

**What**

* Write logic **once**
* Reuse it everywhere

**Why**

* Less copy-paste
* Fewer bugs
* Easy updates

**In Jenkins**

* Without shared lib → same pipeline copied in 10 repos ❌
* With shared lib → one pipeline used by 10 repos ✅

---

## Why Jenkins Shared Libraries (What & Why)

### What

* A **central Git repo** that contains reusable Jenkins pipeline logic

### Why

* Enforce same CI standards everywhere
* Security rules written once
* Jenkinsfiles stay small

### Benefits

* DRY pipelines
* Easy governance
* Faster onboarding
* One change affects all pipelines

---

## Folder Structure – Only Insights

```
jenkins-shared-lib/
├── vars/
├── src/
└── resources/
```

---

## vars/

**What**

* Entry point for pipelines
* Files here behave like Jenkins steps

**Why**

* Moves full pipeline logic out of Jenkinsfile
* Jenkinsfile becomes just a function call

**Example thinking**

* `platformImageCI()` = complete CI flow

---

## src/

**What**

* Reusable Groovy classes (pure logic)

**Why**

* Keep pipeline readable
* Reuse security / utility logic
* Follow clean code separation

**Example**

* Trivy scan logic
* SBOM generation
* Image signing

---

## resources/

**What**

* Static files (yaml, json, scripts)

**Why**

* Keep configs versioned with pipeline
* Avoid embedding large configs in Groovy

**Used when**

* Tool configs
* Policy files
* Templates

---

## One-line Interview Answer (Gold)

> “We use Jenkins Shared Libraries to follow DRY principles by centralizing pipeline logic in `vars`, reusable code in `src`, and static configs in `resources`, which keeps Jenkinsfiles thin and enforces consistent CI standards.”

---

## 1️⃣ Shared Library Structure (Context)

```
jenkins-shared-lib/
├── vars/
│   └── platformImageCI.groovy
├── src/
│   └── org/security/
│       ├── TrivyScan.groovy
│       └── CosignSign.groovy
└── resources/
    └── trivy.yaml
```

---

## 2️⃣ `vars/` – Full Pipeline Flow

### `vars/platformImageCI.groovy`

```groovy
def call(Map cfg) {
  pipeline {
    agent any
    stages {

      stage('Build') {
        steps {
          sh "docker build -t ${cfg.image} ."
        }
      }

      stage('Scan') {
        steps {
          script {
            org.security.TrivyScan.scan(cfg.image)
          }
        }
      }

      stage('Sign') {
        steps {
          script {
            org.security.CosignSign.sign(cfg.image)
          }
        }
      }

    }
  }
}
```

👉 **This file controls the whole pipeline**

---

## 3️⃣ `src/` – Reusable Logic

### `src/org/security/TrivyScan.groovy`

```groovy
package org.security

class TrivyScan {
  static void scan(String image) {
    sh "trivy image --config trivy.yaml ${image}"
  }
}
```

### `src/org/security/CosignSign.groovy`

```groovy
package org.security

class CosignSign {
  static void sign(String image) {
    sh "cosign sign ${image}"
  }
}
```

👉 **No stages here**
👉 **Only logic**
👉 **Called from `vars/`**

---

## 4️⃣ `resources/` – Tool Config / Policy

### `resources/trivy.yaml`

```yaml
severity:
  - HIGH
  - CRITICAL
exit-code: 1
```

### Loaded inside pipeline

```groovy
def trivyConfig = libraryResource 'trivy.yaml'
writeFile file: 'trivy.yaml', text: trivyConfig
```

👉 Config stored once
👉 Used by all pipelines

---

## 5️⃣ Jenkinsfile – Shared Lib CALL (Very Important)

### Jenkinsfile in Application Repo

```groovy
@Library('jenkins-shared-lib') _

platformImageCI(
  image: "java-platform:v1"
)
```

👉 This is the **actual CALL**
👉 No pipeline code here
👉 DRY achieved

---

## 6️⃣ How to Explain in Interview (1-liner)

> “In Jenkins Shared Libraries, `vars` defines the pipeline flow, `src` holds reusable logic like scans and signing, and `resources` stores tool configs. Jenkinsfiles simply call the shared pipeline.”

---

If you want next:

* ❌ Bad call example (what not to do)
* ✅ Multiple pipelines using same shared lib
* 🧠 How Jenkins resolves `@Library`

Just tell me 👍

