<div align="center">

# 🏢 Infrastructure Architecture Overview
### Arquitectura de Infraestructura

---

</div>

## 📋 Purpose | Propósito

<table>
<tr>
<td width="50%" valign="top">

### 🇧🇴 Español (Bolivia)

Este repositorio define la plataforma completa de **Infraestructura como Código (IaC)** y **GitOps** utilizada para aprovisionar, administrar y observar un ecosistema Kubernetes multiambiente desplegado sobre **AWS EKS** (Elastic Kubernetes Service).

Cumple con los **estándares empresariales 2025**, dando prioridad a:
- Seguridad
- Escalabilidad
- Automatización
- Observabilidad

</td>
<td width="50%" valign="top">

### 🇺🇸 English

This repository defines the complete **Infrastructure as Code (IaC)** and **GitOps-driven platform** used to provision, manage, and observe a multi-environment Kubernetes ecosystem deployed on **AWS EKS** (Elastic Kubernetes Service).

It follows **2025 enterprise standards** emphasizing:
- Security
- Scalability
- Automation
- Observability

</td>
</tr>
</table>

---

## 🎯 Core Principles | Principios Fundamentales

<table>
<tr>
<td width="50%" valign="top">

### 🇧🇴 Español (Bolivia)

- **Operación Declarativa**  
  Todos los componentes (infraestructura, cargas, políticas) se definen como código y se sincronizan usando GitOps.

- **Paridad de Ambientes**  
  Configuración consistente entre desarrollo, staging y producción.

- **Separación de Responsabilidades**  
  La plataforma (infra) y las aplicaciones (services) están aisladas para escalabilidad y seguridad.

- **Cero Deriva Manual**  
  El cluster refleja el estado de Git: todos los cambios pasan por PRs.

</td>
<td width="50%" valign="top">

### 🇺🇸 English

- **Declarative Operations**  
  All components (infrastructure, workloads, policies) are defined as code and synchronized through GitOps.

- **Environment Parity**  
  Consistent configuration across development, staging, and production.

- **Separation of Concerns**  
  Platform (infra) and application (services) are isolated for scalability and security.

- **Zero Manual Drift**  
  The cluster is the reflection of the Git state — all changes flow through PRs.

</td>
</tr>
</table>

---

## 📁 Repository Structure | Estructura del Repositorio

```
infra/
├─ terraform/        → Provisioning (EKS, VPC, RDS, ECR, IAM, KMS)
├─ clusters/         → GitOps state per environment (dev/stg/prod)
│  ├─ apps/          → Argo CD Applications & Helm references
│  ├─ values/        → Global and environment overrides
│  └─ helmfile/      → Declarative Helm orchestration
├─ charts/           → Helm charts (add-ons & umbrella)
├─ policies/         → Security baselines (Kyverno/Gatekeeper)
├─ secrets/          → External Secrets templates & backends
├─ monitoring/       → Prometheus, Grafana, Loki, Tempo, alert rules
├─ networking/       → Ingress, certificates, network policies
├─ namespaces/       → Namespace definitions by context
├─ cost/             → Budgeting, Kubecost integration
├─ tooling/          → Linters, schemas, pre-commit hooks
├─ pipelines/        → CI/CD automation (Terraform, Argo sync)
└─ docs/             → Operational & architectural documentation
```

<table>
<tr>
<td width="50%" valign="top">

**🇧🇴** La estructura separa la lógica de plataforma, la configuración de ambientes, la observabilidad y la gobernanza. Cada módulo está versionado y manejado por controladores GitOps (Argo CD / Helm Controller).

</td>
<td width="50%" valign="top">

**🇺🇸** The structure separates platform logic, environment configuration, observability, and governance. Each module is independently versioned and managed by GitOps controllers (Argo CD / Helm Controller).

</td>
</tr>
</table>

---

## ☁️ Cloud & IaC Layer | Capa de Nube e Infraestructura como Código

<table>
<tr>
<td width="50%" valign="top">

### 🇧🇴 Español (Bolivia)

Terraform define los recursos base de AWS:

- **VPC y Subnets**  
  Redes públicas/privadas segmentadas.

- **EKS Cluster**  
  Plano de control gestionado con node groups (On-Demand + Spot).

- **ECR**  
  Registro centralizado de imágenes.

- **RDS/KMS/Secrets Manager**  
  Persistencia y encriptación seguras.

</td>
<td width="50%" valign="top">

### 🇺🇸 English

Terraform defines foundational AWS resources:

- **VPC & Subnets**  
  Segregated public/private networks.

- **EKS Cluster**  
  Managed control plane with node groups (On-Demand + Spot).

- **ECR**  
  Centralized image registry.

- **RDS/KMS/Secrets Manager**  
  Secure persistence and encryption.

</td>
</tr>
</table>

---

## 🔁 GitOps Workflow | Flujo GitOps

<table>
<tr>
<td width="50%" valign="top">

### 🇧🇴 Español (Bolivia)

1. **CI construye y escanea contenedores** → los publica en ECR.

2. **El repositorio infra mantiene el estado deseado** usando Argo CD.

3. **Argo CD sincroniza continuamente** los manifests → cluster.

4. **Los cambios son versionados, revisados y auditables.**

</td>
<td width="50%" valign="top">

### 🇺🇸 English

1. **CI builds and scans containers** → pushes to ECR.

2. **Infra repo tracks desired state** via Argo CD Applications.

3. **Argo CD continuously reconciles** manifests → cluster.

4. **Changes are versioned, reviewed, and auditable.**

</td>
</tr>
</table>

---

## 🛡️ Security Framework | Marco de Seguridad

<table>
<tr>
<td width="50%" valign="top">

### 🇧🇴 Español (Bolivia)

- **Políticas de Ejecución**  
  Kyverno y OPA aplican el baseline de seguridad (no-root, FS de solo lectura, probes, límites).

- **Control de Red**  
  Aislamiento por namespace, NetworkPolicies y malla de servicios opcional con mTLS.

- **Manejo de Secretos**  
  External Secrets Operator (AWS Secrets Manager/Vault).

- **Cumplimiento**  
  Generación de SBOM, escaneo de imágenes y control de cambios vía Git.

</td>
<td width="50%" valign="top">

### 🇺🇸 English

- **Runtime Policies**  
  Kyverno & OPA enforce pod security baseline (non-root, read-only FS, probes, limits).

- **Network Control**  
  Namespace isolation, NetworkPolicies, and optional service mesh for mTLS.

- **Secrets Management**  
  External Secrets Operator (AWS Secrets Manager/Vault).

- **Compliance**  
  SBOM generation, image scanning, and Git-based change control.

</td>
</tr>
</table>

---

## 📊 Observability Stack | Pila de Observabilidad

<table>
<tr>
<td width="50%" valign="top">

### 🇧🇴 Español (Bolivia)

- **Métricas**  
  Prometheus + ServiceMonitors personalizados.

- **Logs**  
  Loki (logging agregado multi-namespace).

- **Trazas**  
  OpenTelemetry → backend Tempo o Jaeger.

- **Tableros**  
  Grafana con tableros y alertas versionados.

- **Alertas**  
  PrometheusRules + Alertmanager routing.

</td>
<td width="50%" valign="top">

### 🇺🇸 English

- **Metrics**  
  Prometheus + custom ServiceMonitors.

- **Logs**  
  Loki (aggregated multi-namespace logging).

- **Traces**  
  OpenTelemetry → Tempo or Jaeger backend.

- **Dashboards**  
  Grafana with version-controlled dashboards and alerts.

- **Alerting**  
  PrometheusRules + Alertmanager routing.

</td>
</tr>
</table>

---

## 🌐 Networking & Ingress | Red y Entrada

<table>
<tr>
<td width="50%" valign="top">

### 🇧🇴 Español (Bolivia)

- **Ingress Controllers**  
  AWS Load Balancer Controller (ALB/NLB) + Cert-Manager.

- **Automatización TLS**  
  Integración con ACM para certificados HTTPS.

- **Multi-Gateway**  
  Ingress interno/privado para tráfico del sistema e ingress público para APIs.

</td>
<td width="50%" valign="top">

### 🇺🇸 English

- **Ingress Controllers**  
  AWS Load Balancer Controller (ALB/NLB) + Cert-Manager.

- **TLS Automation**  
  ACM integration for HTTPS certificates.

- **Multi-Gateway Setup**  
  Internal/private ingress for system traffic and public ingress for APIs.

</td>
</tr>
</table>

---

## ⚙️ Scalability & Reliability | Escalabilidad y Confiabilidad

<table>
<tr>
<td width="50%" valign="top">

### 🇧🇴 Español (Bolivia)

- **HPA**  
  Para servicios stateless.

- **Escalado por eventos mediante KEDA**  
  Para colas/streams.

- **PDB**  
  Para alta disponibilidad.

- **Cluster Autoscaler**  
  Para elasticidad de nodos.

- **Rollouts Blue/Green y Canary**  
  Con Argo Rollouts.

</td>
<td width="50%" valign="top">

### 🇺🇸 English

- **Horizontal Pod Autoscaler (HPA)**  
  For stateless services.

- **Event-based autoscaling via KEDA**  
  For queue/stream workloads.

- **Pod Disruption Budgets (PDB)**  
  For high availability.

- **Cluster Autoscaler**  
  For node group elasticity.

- **Blue/Green & Canary Rollouts**  
  Via Argo Rollouts.

</td>
</tr>
</table>

---

## 📜 Compliance & Governance | Cumplimiento y Gobernanza

<table>
<tr>
<td width="50%" valign="top">

### 🇧🇴 Español (Bolivia)

- **Validación Pre-merge**  
  Todos los manifests validados (política + esquema).

- **Pruebas de Cumplimiento**  
  Conftest y OPA para pruebas de cumplimiento.

- **Auditorías**  
  Registradas en Git y eventos de Argo CD.

- **Documentación**  
  Runbooks versionados en `infra/docs/`.

</td>
<td width="50%" valign="top">

### 🇺🇸 English

- **Pre-merge Validation**  
  All manifests validated (policy + schema checks).

- **Compliance Testing**  
  Conftest & OPA for compliance testing.

- **Audit Trails**  
  Maintained through Git and Argo CD events.

- **Documentation**  
  Runbooks versioned in `infra/docs/`.

</td>
</tr>
</table>

---

## 🧰 Toolchain | Cadena de Herramientas

| **Domain** | **Tools** |
|------------|----------|
| **IaC** | Terraform, AWS CLI |
| **GitOps** | Argo CD, Helm |
| **Observability** | Prometheus, Grafana, Loki, Tempo, OTEL |
| **Security** | Kyverno, Gatekeeper, External Secrets, Trivy |
| **CI/CD** | GitHub Actions / GitLab CI |
| **Cost Management** | Kubecost |
| **Operations** | Lens, ArgoCD UI, Grafana |

---

## 💰 AWS Cost Breakdown | Desglose de Costos AWS

<table>
<tr>
<td width="50%" valign="top">

### 🇧🇴 Español (Bolivia)

Esta infraestructura usa **servicios AWS de nivel empresarial** con Rancher para el manejo de Kubernetes, resultando en costos premium debido a:

- **EKS Control Plane**: ~$73/mes por cluster
- **EC2 Node Groups**: Variable según tipos de instancia (On-Demand + Spot)
- **RDS Databases**: Despliegues Multi-AZ para alta disponibilidad
- **Load Balancers**: ALB/NLB para tráfico de ingress
- **Transferencia de Datos**: Cross-AZ y salida a internet
- **ECR Storage**: Registro de imágenes de contenedores
- **Secrets Manager**: Almacenamiento encriptado de secretos
- **CloudWatch**: Retención de logs y métricas
- **Rancher Management**: Orquestación K8s de primera

**¿Por qué el costo?** Usamos **herramientas y servicios de primer nivel** para garantizar máxima confiabilidad, seguridad y escalabilidad.

</td>
<td width="50%" valign="top">

### 🇺🇸 English

This infrastructure uses **enterprise-grade AWS services** with Rancher for Kubernetes management, resulting in premium costs due to:

- **EKS Control Plane**: ~$73/month per cluster
- **EC2 Node Groups**: Variable based on instance types (On-Demand + Spot)
- **RDS Databases**: Multi-AZ deployments for HA
- **Load Balancers**: ALB/NLB for ingress traffic
- **Data Transfer**: Cross-AZ and internet egress
- **ECR Storage**: Container image registry
- **Secrets Manager**: Encrypted secrets storage
- **CloudWatch**: Logs and metrics retention
- **Rancher Management**: Best-in-class K8s orchestration

**Why the cost?** We use **top-tier tools and services** to ensure maximum reliability, security, and scalability.

</td>
</tr>
</table>

---

## 💵 Cost Estimation by Scale | Estimación de Costos por Escala

<table>
<tr>
<td width="50%" valign="top">

### 🇧🇴 Español (Bolivia)

#### 🏠 **Opción Básica - Pocos Usuarios**
*Para startups o proyectos pequeños*

- **EC2 t3.small/medium** con PM2: ~$15-30/mes
- **RDS t3.micro** (PostgreSQL/MySQL): ~$15/mes
- **Load Balancer básico**: ~$16/mes
- **Almacenamiento S3**: ~$5/mes
- **Transferencia de datos**: ~$10/mes

**💰 Total estimado: $60-80/mes**

✅ **Sirve para**: 100-500 usuarios concurrentes  
⚠️ **Limitaciones**: Sin auto-escalado, sin alta disponibilidad, mantenimiento manual

---

#### 🏢 **Opción Enterprise - Alta Escalabilidad**
*Para empresas grandes con miles de usuarios*

- **EKS Control Plane**: $73/mes
- **EC2 Node Groups** (3-10 nodos): $200-600/mes
- **RDS Multi-AZ** (db.r5.large): $150-300/mes
- **Load Balancers** (ALB/NLB): $50-100/mes
- **Rancher + Monitoring**: $100-200/mes
- **ECR, Secrets, CloudWatch**: $50-100/mes
- **Transferencia de datos**: $100-300/mes

**💰 Total estimado: $700-1,700/mes**

✅ **Sirve para**: Miles de usuarios concurrentes  
✅ **Beneficios**: Auto-escalado horizontal, alta disponibilidad 99.9%, recuperación automática, zero-downtime deployments

---

**📞 ¿Empresa grande?** Hablemos de un plan premium personalizado con descuentos por volumen y soporte 24/7.

</td>
<td width="50%" valign="top">

### 🇺🇸 English

#### 🏠 **Basic Option - Few Users**
*For startups or small projects*

- **EC2 t3.small/medium** with PM2: ~$15-30/month
- **RDS t3.micro** (PostgreSQL/MySQL): ~$15/month
- **Basic Load Balancer**: ~$16/month
- **S3 Storage**: ~$5/month
- **Data Transfer**: ~$10/month

**💰 Estimated Total: $60-80/month**

✅ **Good for**: 100-500 concurrent users  
⚠️ **Limitations**: No auto-scaling, no high availability, manual maintenance

---

#### 🏢 **Enterprise Option - High Scalability**
*For large companies with thousands of users*

- **EKS Control Plane**: $73/month
- **EC2 Node Groups** (3-10 nodes): $200-600/month
- **RDS Multi-AZ** (db.r5.large): $150-300/month
- **Load Balancers** (ALB/NLB): $50-100/month
- **Rancher + Monitoring**: $100-200/month
- **ECR, Secrets, CloudWatch**: $50-100/month
- **Data Transfer**: $100-300/month

**💰 Estimated Total: $700-1,700/month**

✅ **Good for**: Thousands of concurrent users  
✅ **Benefits**: Horizontal auto-scaling, 99.9% high availability, automatic recovery, zero-downtime deployments

---

**📞 Large enterprise?** Let's discuss a custom premium plan with volume discounts and 24/7 support.

</td>
</tr>
</table>

---
## 🚀 Deployment Strategy | Estrategia de Despliegue

<table>
<tr>
<td width="50%" valign="top">

### 🇧🇴 Español (Bolivia)

1. **El pipeline CI** construye → escanea → publica en ECR.

2. **La automatización de PR** actualiza tags en `infra/clusters/<env>/apps/`.

3. **Argo CD** sincroniza y valida el estado del rollout.

4. **Rollback o promoción** manejados automáticamente por reglas GitOps.

</td>
<td width="50%" valign="top">

### 🇺🇸 English

1. **CI pipeline** builds → scans → publishes to ECR.

2. **PR automation** updates tags in `infra/clusters/<env>/apps/`.

3. **Argo CD** syncs and validates rollout health.

4. **Rollback or promotion** handled automatically via GitOps rules.

</td>
</tr>
</table>

---

## 🔍 Lens & Operations | Operación con Lens

<table>
<tr>
<td width="50%" valign="top">

### 🇧🇴 Español (Bolivia)

**Lens funciona como el plano visual de control para clusters EKS.**

Permite:

- Inspeccionar pods, deployments y namespaces.
- Ver logs, eventos y métricas en tiempo real.
- Aplicar políticas, observar el estado de Argo CD y monitorear el escalado HPA.

</td>
<td width="50%" valign="top">

### 🇺🇸 English

**Lens acts as the visual control plane for EKS clusters.**

Operators can:

- Inspect pods, deployments, and namespaces.
- View logs, events, and metrics in real time.
- Apply policies, watch Argo CD health, and monitor HPA scaling.

</td>
</tr>
</table>

---

## 📚 Documentation | Documentación

<table>
<tr>
<td width="50%" valign="top">

### 🇧🇴 Español (Bolivia)

Toda la documentación arquitectónica y operativa se encuentra en `/docs`:

- `platform-overview.md`
- `gitops-flow.md`
- `security-model.md`
- `observability-handbook.md`
- `eks-operations-with-lens.md`

</td>
<td width="50%" valign="top">

### 🇺🇸 English

All architectural and operational documents are under `/docs`:

- `platform-overview.md`
- `gitops-flow.md`
- `security-model.md`
- `observability-handbook.md`
- `eks-operations-with-lens.md`

</td>
</tr>
</table>

---

## ⚡ Summary | Resumen

<table>
<tr>
<td width="50%" valign="top">

### 🇧🇴 Español (Bolivia)

Una **plataforma de nivel producción, declarativa y basada en GitOps** — que combina aprovisionamiento con Terraform, orquestación con Argo CD y un ambiente EKS reforzado con observabilidad, escalabilidad y cumplimiento integrados.

</td>
<td width="50%" valign="top">

### 🇺🇸 English

A **production-grade, declarative, and GitOps-driven platform** — combining Terraform provisioning, Argo CD orchestration, and a hardened EKS runtime with complete observability, scalability, and compliance baked in.

</td>
</tr>
</table>

---

<div align="center">

---

### 👨‍💻 Architecture & Design

**YovanEnovore**

📧 [yovanuxf@gmail.com](mailto:yovanuxf@gmail.com)  
🌐 [https://ynvbo.site](https://ynvbo.site)

---

**Built with ❤️ for Enterprise-Grade Infrastructure YNVBO-CORP**

*Powered by YovanEnovore*

</div>
