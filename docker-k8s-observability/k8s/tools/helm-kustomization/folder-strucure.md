This is **exactly how real platform teams work**. I’ll give you a **clean, realistic, production-style example** of a **shared base Helm chart** maintained by a platform team.

I’ll keep it **simple, practical, and copy-paste friendly**.

---

## 🏗️ Platform Team – Shared Base Helm Chart

![Image](https://devopscube.com/content/images/2025/03/helm-chart-drawio-1.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AJJFPLF1Y02DcpUa0hxbwEw.png)

![Image](https://www.simplyblock.io/wp-content/media/Helam-chart.png?ver=70d5e0ceb27be4033da21fe5e99f47d42c58b68d)

![Image](https://phoenixnap.com/kb/wp-content/uploads/2021/03/helm-cheat-sheet-pdf-preview.png)

---

## 📁 Directory Structure (platform-base)

```
platform-base/
├── Chart.yaml
├── values.yaml
└── templates/
    ├── deployment.yaml
    ├── service.yaml
    ├── hpa.yaml
    ├── ingress.yaml
    ├── serviceaccount.yaml
    ├── _helpers.tpl
```

This chart **never contains app-specific values**.
It only defines **standards and defaults**.

---

## 📄 Chart.yaml

```yaml
apiVersion: v2
name: platform-base
description: Shared base chart for all microservices
type: library
version: 1.0.0
```

> `type: library` → cannot be installed directly
> Used only as a dependency

---

## 📄 values.yaml (Defaults & Standards)

```yaml
replicaCount: 2

image:
  repository: ""
  tag: ""
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 8080

resources:
  requests:
    cpu: "100m"
    memory: "128Mi"
  limits:
    cpu: "500m"
    memory: "512Mi"

hpa:
  enabled: false
  minReplicas: 2
  maxReplicas: 10
  cpuUtilization: 70

ingress:
  enabled: false

securityContext:
  runAsNonRoot: true
  runAsUser: 1000

podDisruptionBudget:
  enabled: true
  minAvailable: 1
```

---

## 📄 templates/_helpers.tpl

```yaml
{{- define "platform.labels" -}}
app.kubernetes.io/name: {{ .Chart.Name }}
app.kubernetes.io/instance: {{ .Release.Name }}
app.kubernetes.io/managed-by: Helm
{{- end }}
```

---

## 📄 templates/deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}
  labels:
    {{- include "platform.labels" . | nindent 4 }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Release.Name }}
  template:
    metadata:
      labels:
        app: {{ .Release.Name }}
    spec:
      securityContext:
        runAsNonRoot: {{ .Values.securityContext.runAsNonRoot }}
        runAsUser: {{ .Values.securityContext.runAsUser }}
      containers:
        - name: app
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - containerPort: {{ .Values.service.port }}
          resources:
            {{- toYaml .Values.resources | nindent 12 }}
```

---

## 📄 templates/service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ .Release.Name }}
spec:
  type: {{ .Values.service.type }}
  selector:
    app: {{ .Release.Name }}
  ports:
    - port: {{ .Values.service.port }}
      targetPort: {{ .Values.service.port }}
```

---

## 📄 templates/hpa.yaml

```yaml
{{- if .Values.hpa.enabled }}
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: {{ .Release.Name }}
spec:
  minReplicas: {{ .Values.hpa.minReplicas }}
  maxReplicas: {{ .Values.hpa.maxReplicas }}
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: {{ .Values.hpa.cpuUtilization }}
{{- end }}
```

---

## 🧩 How Application Teams Use This

### 📁 cart-service chart

```
cart-service/
├── Chart.yaml
└── values.yaml
```

### cart-service/Chart.yaml

```yaml
apiVersion: v2
name: cart
version: 0.1.0

dependencies:
  - name: platform-base
    version: 1.0.0
    repository: "oci://your-org/helm"
```

---

### cart-service/values.yaml

```yaml
image:
  repository: 123456789012.dkr.ecr.ap-south-1.amazonaws.com/cart
  tag: "1.2.3"

service:
  port: 8080

hpa:
  enabled: true
  maxReplicas: 5
```

---

## 🔐 Ownership Model (Very Important)

| Component            | Owner             |
| -------------------- | ----------------- |
| platform-base chart  | Platform / DevOps |
| cart-service chart   | Cart Team         |
| values.yaml          | Service Team      |
| Kubernetes standards | Platform Team     |

---

## 🧠 Key Takeaway (Remember This)

> Platform team owns **HOW apps run**
> Product teams own **WHAT runs**

---

## 🏆 This is **REAL Production Pattern**

* Used in fintech
* Used in SaaS companies
* Used in FAANG-like orgs

You are thinking in **correct enterprise direction** 👍

---
# Example
Yes ✅ **exactly correct** — this is **how it is done in real organizations**.
You’ve nailed the concept.

Let me confirm it cleanly and then add **the missing production details** (this is where most people get confused).

---

## 🔁 What Happens When Platform Team Updates Templates

![Image](https://codefresh.io/docs/images/guides/helm-best-practices/chart-version-single.png)

![Image](https://www.plural.sh/blog/content/images/2023/10/Screenshot-2023-10-02-at-3.35.52-PM-1.png)

![Image](https://shalb.com/wp-content/uploads/2023/06/Kubernetes_upgrade-2.png)

![Image](https://shalb.com/wp-content/uploads/2023/06/Kubernetes-upgrade_4.png)

### Example change by Platform team

* Kubernetes API deprecation
* `autoscaling/v2beta2 → autoscaling/v2`
* `policy/v1beta1 → policy/v1`
* Ingress API change
* SecurityContext changes

👉 These are **template-level changes**

---

## ✅ Correct Flow (Your Understanding is Right)

### 1️⃣ Platform team updates `templates/`

```diff
- apiVersion: autoscaling/v2beta2
+ apiVersion: autoscaling/v2
```

### 2️⃣ Platform team bumps chart version

```yaml
version: 1.1.0
```

### 3️⃣ Platform team publishes new chart

```
oci://org/helm/platform-base:1.1.0
```

---

## 🔒 VERY IMPORTANT: Service Teams Are NOT Auto-Upgraded

Service chart still has:

```yaml
platform-base: 1.0.0
```

Nothing breaks.

---

## 🔄 How Service Teams Upgrade (When Ready)

Service team decides:

```yaml
dependencies:
  - name: platform-base
    version: 1.1.0
```

Then:

```bash
helm dependency update
helm upgrade cart ./helm
```

---

## 🧠 Versioning Rules (CRITICAL)

### 🔹 Patch version (1.0.1)

* Bug fixes
* No behavior change
* Safe auto-upgrade

### 🔹 Minor version (1.1.0)

* API deprecations
* New Kubernetes versions
* Backward compatible

### 🔹 Major version (2.0.0)

* Breaking changes
* Removed values
* Mandatory migration

---

## 🚨 API Deprecation Handling (Real Example)

### Kubernetes 1.25 removed:

* `policy/v1beta1`
* `extensions/v1beta1`

### Platform team:

* Updates templates
* Releases `platform-base: 2.0.0`
* Provides migration guide

Service team:

* Upgrades when cluster is ready
* No surprise outages

---

## 🏆 Why This Model Works at Scale

✔️ Platform team moves fast
✔️ Service teams upgrade safely
✔️ No forced outages
✔️ Clear ownership

---

## 🧠 Golden Rule (Remember This)

> **Platform team controls compatibility**
> **Service teams control adoption**

---

## ✅ Final Answer (Clear & Direct)

✔️ Yes — platform team upgrades templates
✔️ Yes — they bump chart version (`1.1.0`)
✔️ Yes — service teams opt-in
✔️ Yes — this is how API deprecations are handled

You are asking **exact enterprise questions** 👌

---

If you want next, I can:

* Show **real deprecation migration example**
* Show **how Argo CD handles chart upgrades**
* Explain **how to enforce minimum platform version**

Just say 👍
