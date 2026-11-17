# 🍽️ Zomato Clone — End-to-End DevSecOps Pipeline & Kubernetes Deployment

A full-stack food delivery web application (Zomato Clone) packaged and deployed using an enterprise-grade **DevSecOps CI/CD pipeline**, containerized with **Docker**, orchestrated on **Kubernetes (AWS EKS)**, monitored with **Prometheus & Grafana**, and continuously delivered via **ArgoCD**.

---

## 📌 Architecture & Workflow

```mermaid
flowchart TD
    A[👨‍💻 Code Commit & Push] --> B[⚙️ Jenkins CI Pipeline]
    B --> C[🧹 Clean Workspace & Checkout]
    B --> D[🔍 SonarQube Code Analysis]
    D --> E[🛡️ Code Quality Gate]
    B --> F[📦 Install Dependencies]
    F --> G[🔒 OWASP Dependency Check]
    B --> H[🛡️ Trivy Filesystem Scan]
    H --> I[🐳 Docker Build Image]
    I --> J[🏷️ Tag & Push to DockerHub]
    J --> K[🔬 Docker Scout Vulnerability Scan]
    K --> L[🚀 Deploy to Docker / K8s Cluster]
    L --> M[☸️ Kubernetes EKS Cluster]
    M --> N[📊 Prometheus & Grafana Monitoring]
    M --> O[🔄 ArgoCD GitOps Sync]
    L --> P[📧 Automated Email Alerts]
```

---

## 🛠️ Tech Stack & DevOps Tooling

| Domain | Technologies / Tools |
|---|---|
| **Frontend Application** | React 18, Material UI (MUI), Sass (SCSS), Web Vitals |
| **Containerization** | Docker, Dockerfile (Node 16 slim multi-stage) |
| **Continuous Integration** | Jenkins (Declarative Pipeline with JDK 17 & Node 23) |
| **Static Code Analysis** | SonarQube Scanner & Sonar Quality Gates |
| **Security & Vulnerability Scanning** | OWASP Dependency-Check, Trivy (FS scan), Docker Scout |
| **Container Registry** | DockerHub |
| **Container Orchestration** | Kubernetes, AWS EKS, `kubectl`, `eksctl` |
| **Continuous Delivery / GitOps** | ArgoCD |
| **Monitoring & Observability** | Prometheus, Node Exporter, Grafana |
| **Infrastructure & Cloud** | AWS (EC2 Ubuntu 24.04, EKS, CloudFormation) |

---

## 🚀 What Was Implemented

### 1. 🎨 Frontend Application (React & Sass)
- Developed a responsive **Zomato landing clone** featuring:
  - Dynamic navigation header with location picker and search functionality.
  - Category cards for delivery, dining out, and nightlife experiences.
  - Curated dining collection showcases and locality explorer grid.
  - Interactive FAQ accordion sections with expandable question cards.
  - App download CTA banner linking to Google Play and Apple App Store.
  - Modular Sass architecture with dedicated component styles and responsive breakpoints.

### 2. 🐳 Containerization (Docker)
- Authored an optimized [Dockerfile](file:///d:/PROJECT/DevOps-Project-Zomato/Dockerfile) utilizing `node:16-slim` as the base image.
- Implemented efficient layer caching by copying `package.json` and `package-lock.json` before application source code.
- Production build compilation using `npm run build` with application served on port `3000`.

### 3. ⚙️ Jenkins CI/CD & DevSecOps Pipeline
Built a robust declarative [jenkinsfile](file:///d:/PROJECT/DevOps-Project-Zomato/jenkinsfile) covering the complete software delivery lifecycle:
- **Clean Workspace & SCM Checkout**: Automatically sanitizes previous build artifacts and fetches the latest codebase.
- **SonarQube Static Analysis**: Scans source code for code smells, bugs, and duplications.
- **SonarQube Quality Gate**: Pipeline enforces quality standards and halts execution if quality criteria fail.
- **OWASP Dependency-Check**: Performs Software Composition Analysis (SCA) to detect known CVEs in third-party npm packages.
- **Trivy Vulnerability Scanner**: Executes deep filesystem scans for misconfigurations and security risks, publishing `trivy.txt` reports.
- **Docker Build & Push**: Builds the container image, tags it with proper versioning, and pushes to DockerHub securely via credentials store.
- **Docker Scout Security Analysis**: Runs image quickview, CVE analysis, and actionable remediation recommendations.
- **Automated Deployment**: Spins up the containerized application on target environment.
- **Post-Build Email Notifications**: Transmits HTML summary emails with build status, job metadata, and attached Trivy scan reports upon completion.

### 4. ☸️ Kubernetes (AWS EKS) Orchestration
- Created declarative Kubernetes manifests under [Kubernetes/](file:///d:/PROJECT/DevOps-Project-Zomato/Kubernetes):
  - **Deployment** ([deployment.yaml](file:///d:/PROJECT/DevOps-Project-Zomato/Kubernetes/deployment.yaml)): Manages 2 pod replicas of the Zomato application with automated rollout and restart policies.
  - **Application Service** ([service.yaml](file:///d:/PROJECT/DevOps-Project-Zomato/Kubernetes/service.yaml)): Exposes the application using a `NodePort` service mapped to port `30001` (targetPort `3000`).
  - **Node Exporter Service** ([node-service.yaml](file:///d:/PROJECT/DevOps-Project-Zomato/Kubernetes/node-service.yaml)): Configures the metric exporter service on port `9100` within the `prometheus-node-exporter` namespace.

### 5. 📈 Monitoring, Observability & GitOps
- **Prometheus & Grafana**: Deployed Prometheus Node Exporter via Helm to collect system and host-level metrics. Configured scrape jobs targeting cluster nodes on port `9100`.
- **ArgoCD**: Implemented GitOps continuous delivery by deploying ArgoCD server with LoadBalancer ingress, synchronizing Kubernetes cluster state directly with Git.

---

## 📂 Project Directory Structure

```text
DevOps-Project-Zomato/
├── .gitignore                      # Git ignore configuration
├── Command.txt                     # Comprehensive DevSecOps deployment runbook
├── Dockerfile                      # Node.js containerization specifications
├── jenkinsfile                     # Multi-stage DevSecOps CI/CD pipeline
├── package.json                    # Application metadata and dependencies
├── package-lock.json               # Deterministic dependency lockfile
├── public/                         # Static assets, HTML template, favicon & manifest
│   ├── favicon.ico
│   ├── index.html
│   └── manifest.json
├── src/                            # Application source code
│   ├── App.js                      # Root application component
│   ├── app.scss                    # Global layout styling
│   ├── data.js                     # Mock restaurant, locality, and FAQ datasets
│   ├── index.js                    # React DOM entry point
│   ├── assets/images/              # Application graphics, banners, and logos
│   └── components/                 # Modular React UI components
│       ├── Header/                 # Navigation bar and hero search
│       ├── Card/                   # Category experience cards
│       ├── Collections/            # Curated dining collection carousel
│       ├── Cities/                 # Popular localities explorer
│       ├── Accordian/              # Expandable FAQ items
│       ├── AccContainer/           # FAQ section container
│       ├── CTA/                    # Mobile app download section
│       └── Footer/                 # Footer links and social indicators
└── Kubernetes/                     # Kubernetes deployment configurations
    ├── deployment.yaml             # App deployment manifest (2 replicas)
    ├── service.yaml                # App NodePort service (port 30001)
    └── node-service.yaml           # Prometheus Node Exporter service
```

---

## 🚦 Getting Started

### Local Development (Frontend)

1. **Clone the repository:**
   ```bash
   git clone <repo-url>
   cd DevOps-Project-Zomato
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Start local development server:**
   ```bash
   npm start
   ```
   The application will be accessible at `http://localhost:3000`.

---

### Running via Docker

1. **Build the Docker image:**
   ```bash
   docker build -t zomato:latest .
   ```

2. **Run the container:**
   ```bash
   docker run -d --name zomato -p 3000:3000 zomato:latest
   ```

3. Open your browser and navigate to `http://localhost:3000`.

---

### Kubernetes Deployment (EKS)

1. **Apply deployment and service manifests:**
   ```bash
   kubectl apply -f Kubernetes/deployment.yaml
   kubectl apply -f Kubernetes/service.yaml
   ```

2. **Verify pods and services:**
   ```bash
   kubectl get pods -l app=zomato
   kubectl get svc zomato
   ```

3. **Access application:**
   Access the application via your cluster node's IP address on port `30001`:
   ```text
   http://<NODE_PUBLIC_IP>:30001
   ```

---

## 📊 Key Metrics & Observability Telemetry

The infrastructure and application track comprehensive performance, reliability, and security metrics across every layer of the stack:

### 1. Host & Infrastructure Metrics (Prometheus & Node Exporter on Port 9100)
- **CPU Utilization**: Scraped via `node_cpu_seconds_total`, computing active CPU load:
  $$\text{CPU Usage (\%)} = 100 - \left(\text{avg by (instance)} \left(\text{rate}\left(\text{node\_cpu\_seconds\_total}\{\text{mode}="idle"\}[5m]\right)\right) \times 100\right)$$
- **Memory Saturation & Availability**: Evaluated using `node_memory_MemTotal_bytes` and `node_memory_MemAvailable_bytes`:
  $$\text{Memory Usage (\%)} = \frac{\text{MemTotal} - \text{MemAvailable}}{\text{MemTotal}} \times 100$$
  Tracks buffer, cache, and active RAM allocation to prevent Out-Of-Memory (OOM) conditions.
- **Disk I/O & Storage Consumption**: Monitored via `node_filesystem_size_bytes` and `node_filesystem_free_bytes`:
  $$\text{Disk Utilization (\%)} = \frac{\text{Total Size} - \text{Free Size}}{\text{Total Size}} \times 100$$
  Includes disk read/write throughput (`node_disk_read_bytes_total`, `node_disk_written_bytes_total`) and I/O operation rates.
- **Network Throughput & Error Rates**: Monitored via `node_network_receive_bytes_total` and `node_network_transmit_bytes_total`, tracking bandwidth consumption, packet transmission rates, and network drops (`node_network_receive_drop_total`).
- **System Load & Uptime**: Scraped via `node_load1`, `node_load5`, and `node_load15` to assess system saturation relative to CPU core count, alongside system uptime (`node_boot_time_seconds`).

### 2. CI/CD Pipeline & Delivery Telemetry (Jenkins Prometheus Exporter)
- **Build Frequency & Success Rate**: Tracked via `jenkins_builds_success_build_count_total` and `jenkins_builds_failed_build_count_total` to measure pipeline stability and failure recovery.
- **Pipeline Stage Latency**: Measures execution time per stage (SonarQube analysis, OWASP SCA, Trivy scan, Docker build, Docker Scout scan, and deployment).
- **Queue Latency & Waiting Time**: Monitored via `jenkins_queue_size_value` to track build queuing delays and scheduling efficiency.
- **Executor Utilization**: Monitored via `jenkins_executor_in_use_value` and `jenkins_executor_free_value` to measure CI runner capacity and prevent build bottlenecks.

### 3. Kubernetes Cluster Health & Workload Availability (AWS EKS & Prometheus)
- **Scrape Target Availability**: Monitored via `up{job="k8s"}` and `up{job="node_exporter"}` ensuring continuous `1/1 up` health across all nodes and endpoints.
- **Pod Replica Status**: Tracks active vs. desired replicas (configured to 2 replicas in [deployment.yaml](file:///d:/PROJECT/DevOps-Project-Zomato/Kubernetes/deployment.yaml)) ensuring zero-downtime rolling updates.
- **Elastic Auto-Scaling**: Managed Node Group auto-scaling limits verified between 2 minimum and 4 maximum worker nodes (`t3.medium`).
- **Service Ingress & Routing**: Service health verified on NodePort `30001` and ArgoCD AWS LoadBalancer hostname endpoints.

### 4. DevSecOps Security, Quality & Vulnerability Metrics
- **SonarQube Static Analysis**:
  - Code Smells, Bugs, and Security Hotspots counts.
  - Duplication percentage and technical debt remediation estimates.
  - SonarQube Quality Gate status (hard Pass/Fail gating criteria).
- **OWASP Dependency-Check (SCA)**:
  - Vulnerability severity breakdown: Critical, High, Medium, and Low CVE counts.
  - CVSS impact score evaluation across npm dependency tree (`dependency-check-report.xml`).
- **Trivy Vulnerability Scanner**:
  - Filesystem vulnerability density and OS package CVE counts compiled into `trivy.txt`.
- **Docker Scout Image Vulnerabilities**:
  - Container image vulnerability counts across Critical, High, Medium, and Low severity.
  - Base image vulnerability delta and targeted package upgrade recommendations.

### 5. Frontend Performance & Core Web Vitals
- **Cumulative Layout Shift (CLS)**: Measures visual layout stability during page load.
- **Largest Contentful Paint (LCP)**: Evaluates perceived load speed and hero image rendering.
- **First Input Delay (FID)**: Tracks input responsiveness and UI interaction readiness.
- **First Contentful Paint (FCP)**: Measures duration to the first render of DOM content.
- **Time to First Byte (TTFB)**: Measures network request and server response latency.

---

## ⭐ Key Engineering Highlights & Deliverables

- **Automated Multi-Stage DevSecOps Pipeline**: Designed and implemented an end-to-end declarative Jenkins CI/CD pipeline orchestrating 10 sequential stages, including automated workspace sanitization, multi-layer security auditing, container builds, registry publishing, and target deployment.
- **Shift-Left Security Architecture**: Embedded automated security controls early in the delivery lifecycle, incorporating SonarQube static analysis with strict Quality Gates, OWASP Dependency-Check for SCA CVE analysis, Trivy filesystem scans, and Docker Scout container image vulnerability evaluations.
- **Production Kubernetes Infrastructure on AWS EKS**: Provisioned a multi-AZ Amazon EKS cluster (`zomatocluster`) using `eksctl`, integrating IAM OIDC provider roles, managed node groups with auto-scaling capabilities (2 to 4 `t3.medium` worker nodes), and public subnet routing.
- **Declarative Continuous Delivery via GitOps**: Integrated ArgoCD with AWS LoadBalancer ingress, establishing automated synchronization between the Git repository and the Kubernetes cluster for declarative, drift-free deployment workflows.
- **Full-Stack Observability & Metrics Infrastructure**: Deployed Prometheus and Node Exporter via Helm, configuring automated scraping for host infrastructure, Jenkins CI/CD build metrics, and EKS cluster nodes, visualized through customized Grafana dashboards.
- **Optimized Containerization**: Authored a lean Docker configuration using `node:16-slim`, implementing deterministic dependency caching (`package.json` and `package-lock.json`) to minimize build duration and minimize container image attack surface.
- **Automated Incident & Status Reporting**: Configured Jenkins email telemetry delivering responsive HTML notifications upon build completion, detailing job name, build number, URL links, and attached Trivy vulnerability scan reports for proactive triage.

---

## 🔒 Security Best Practices Implemented

- **Static Analysis (SAST)**: SonarQube ensures adherence to code cleanliness and security guidelines before artifact creation.
- **Dependency Scanning (SCA)**: OWASP checks identify vulnerable dependencies before packaging.
- **Image Scanning**: Trivy and Docker Scout verify that base image vulnerabilities and package CVEs are caught prior to registry publishing.
- **Least-Privilege Isolation**: Minimal Node slim image reduces the attack surface by eliminating unnecessary binaries and utilities.
- **Automated Quality Gates**: Builds automatically fail if critical security vulnerabilities or failing quality thresholds are detected.

---

## 📝 License & Acknowledgments

This project is open-source and intended for learning, demonstration, and production-grade DevSecOps practice.
