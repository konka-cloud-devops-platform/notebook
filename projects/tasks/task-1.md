# Creating Platform image
```bash
            Checkout
                ↓
            Hadolint
                ↓
            Docker Build
                ↓
            Trivy Scan
                ↓
            Generate SBOM
                ↓
            Archive SBOM
                ↓
            Docker Push
                ↓
            Cosign Sign[not implemented]
```

1. Create the repo called platform images
```bash
        platoform-images/
        |-- README.md
        |-- java
        |   |-- Dockerfile
        |   `-- Jenkinsfile
        |-- nginx
        |   |-- Dockerfile
        |   `-- Jenkinsfile
        |-- nodejs
        |   |-- Dockerfile
        |   `-- Jenkinsfile
        `-- python
            |-- Dockerfile
            `-- Jenkinsfile
```
2. Install hadolint,docker,trivy,sbom[syft] in the build agent
3. Develop Sharedlibs
```bash
        cicd-platform/
        |-- README.md
        |-- resources
        |   `-- trivy.yaml
        |-- src
        |   `-- org
        |       `-- security
        |           |-- CosignSign.groovy
        |           |-- SBOM.groovy
        |           `-- TrivyScan.groovy
        `-- vars
            `-- platformImageCI.groovy
```
4. Configure Global Pipelines
5. Create folder called platform images in Jenkins
   - under create jenkins-platform-image,node-platform-image,python-platform-image and nginx-platform-image
   - configure pipelines


---

# Your Pipeline (context)

```
Checkout
↓
Hadolint
↓
Docker Build
↓
Trivy Scan
↓
Generate SBOM
↓
Archive SBOM
↓
Docker Push
↓
Cosign Sign
```

This pipeline answers **four different security questions**.

---

# 1️⃣ Hadolint – *“Is my Dockerfile written correctly?”*

### What Hadolint is

* A **Dockerfile linter**
* Checks Dockerfile **before building the image**
* Static analysis (no image build)

### What Hadolint checks

* Bad Docker practices
* Security anti-patterns
* Inefficient instructions

Examples:

* Using `latest` tag
* Running as root
* Missing `USER`
* Bad `RUN` chaining
* Unsafe `curl | bash`

### What Hadolint does NOT do

❌ Does not scan image vulnerabilities
❌ Does not scan OS packages
❌ Does not modify Dockerfile

### Why it is important

* Catches mistakes **early**
* Prevents insecure image design
* Cheap and fast

### Simple definition

> **Hadolint checks “how you build the image.”**

---

# 2️⃣ Trivy – *“Does my image contain known vulnerabilities?”*

### What Trivy is

* A **container image vulnerability scanner**
* Scans the **built image**
* Uses CVE databases

### What Trivy scans

* OS packages (apk, rpm, deb)
* Language packages (pip, npm, maven, etc.)
* Base image vulnerabilities

### Typical Trivy policy (platform standard)

* Fail build only on **CRITICAL**
* Allow LOW / MEDIUM / HIGH with tracking

Example:

```bash
trivy image --severity CRITICAL --exit-code 1 image:tag
```

### What Trivy does NOT do

❌ Does not fix vulnerabilities
❌ Does not sign images
❌ Does not generate dependency inventory

### Simple definition

> **Trivy checks “what vulnerabilities are inside the image.”**

---

# 3️⃣ SBOM – *“What exactly is inside this image?”*

### What SBOM is

* **Software Bill of Materials**
* A **list of everything inside the image**
* Generated using tools like **Syft**

### What SBOM contains

* OS packages
* Libraries
* Versions
* Licenses
* Dependency tree

### Why SBOM exists

* Audits
* Compliance
* Incident response
* Future CVE re-scans (without rebuilding image)

### Important rule

* SBOM is **informational**
* It does **NOT fail the build**

### Why we archive SBOM

* Keep history per build
* Download later
* Map image → dependencies

### Simple definition

> **SBOM answers “what is inside the image.”**

---

# 4️⃣ Cosign – *“Can I trust this image?”*

### What Cosign is

* **Image signing tool**
* Cryptographically signs container images
* Ensures **integrity and provenance**

### When Cosign runs

✔ After image push
❌ Never before push

Why:

* Cosign signs **image digest**
* Digest exists only after push

### What Cosign proves

* Image was built by **your pipeline**
* Image was **not tampered with**
* Image is **trusted**

### What Cosign does NOT do

❌ Does not scan vulnerabilities
❌ Does not change image
❌ Does not reduce CVEs

### Verification

```bash
cosign verify image:tag
```

Result:

* Signed → trusted
* Not signed → rejected

### Simple definition

> **Cosign answers “can I trust this image?”**

---

# 🔗 How all four tools work together (VERY IMPORTANT)

| Tool     | Question it answers                               |
| -------- | ------------------------------------------------- |
| Hadolint | Is the Dockerfile safe and correct?               |
| Trivy    | Are there known vulnerabilities inside the image? |
| SBOM     | What exactly is included in the image?            |
| Cosign   | Can I trust and verify this image?                |

They **do not overlap**.
They **complement each other**.

---

# 🧠 One-line explanation for entire pipeline (interview gold)

> “We lint Dockerfiles, scan images for vulnerabilities, generate SBOMs for transparency, and sign images to ensure supply-chain trust.”

---

# Final takeaway (remember this)

```
Hadolint → Build correctness
Trivy    → Vulnerability detection
SBOM     → Dependency visibility
Cosign   → Image trust
```

You now fully understand **why each stage exists** — not just how to run it.

If you want next:

* One-page README wording
* Interview Q&A from this pipeline
* Diagram explanation
* How companies enforce this in EKS

Just tell me 👍
