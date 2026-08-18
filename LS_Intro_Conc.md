---
marp: true
theme: default
paginate: true
header: '<div style="display:flex; justify-content:space-between; align-items:flex-start; width:100%;"><img src="./slide-assets/CN-SIG-logo.png" height="80px" style="padding-left:1000px"></div>'
style: |
  /* Define global background, subtle gradient, and layout framework */
  section {
    background: linear-gradient(135deg, #fdfbfb 0%, #e4f5f5 100%);
    color: #000000;
    padding: 50px;
    position: relative;
  }
  
  /* Create a left-side vertical accent border on every slide */
  section::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 8px;
    height: 100%;
    background: linear-gradient(to bottom, #008080, #005555);
  }
  /* Style headings for crisp scannability */
  h1 {
    {color: #008080}
    font-weight: 700;
  }
  h2 {
    {color: #008080};
  }

  /* Title slide template - Invert colour */
  section.title-slide::before {
    content: none !important;
  }
  section.title-slide::header {
    content: none !important;
  }
  section.Title {
    background: linear-gradient(135deg, #008080 0%, #003d3d 100%) !important;
    color: #ffffff;
    font-size: 45px;
    h1 {color: #ffffff;}
  }

  section ul, section ol {
    font-size: 24px;
    }
  .centered-image {
      display: block;
      margin-left: auto;
      margin-right: auto;
    }  
  /* Footer typography */
  footer {
    font-size: 0.55em;
    color: #7f8c8d;
    font-weight: 600;
  }

---

<br>

### Cloud-Native SIG present

# Kubernetes for Research Infrastructure: Core Concepts and Practical Use Cases

*20 August 2026, ACIT Hub Webinar*

---

# Agenda

1. Introductions
2. Introduction to Kubernetes
3. Use case 1: Lab in the Cloud
4. Use case 2: Jupyter Notebooks
5. Use case 3: Argo CD and Workflows
6. Summary
7. Q&A

---

<!-- _class: Title -->
<!-- _header: "" -->
# Introductions

---

## Who We Are

This webinar is run by the **Cloud-Native Special Interest Group**, a new community of RSEs/dRTPs for cloud-native technologies in research software and digital infrastructure.

Get involved at [https://cloudnative-sig.ac.uk/](https://cloudnative-sig.ac.uk/)
<br>

Current comittee includes; Laura Shemilt<sup>1</sup>, Alex Lubbock<sup>2</sup>, Piper Fowler-Wright<sup>2</sup>, Lewis Sampson<sup>3</sup>, and Tibor Auer<sup>4</sup>.

We are joined today by Dr Christopher Green<sup>3</sup>, Sys Admin in Scientific Computing Department

<span style="font-size: 0.6em; color: gray; display: block; margin-top: 1em;"><sup>1</sup>BioFAIR, <sup>2</sup>Rosalind Franklin Institute, <sup>3</sup>UKRI STFC DAFNI, <sup>4</sup>University College London</span>


---

## The SIG so far

### We have been;

- Building networks through conference attendance
- Running Kubernetes introduction workshops for RSE/dRTPs
- Sharing knowledge of Kubernetes in the DRI community through CAKE fellowship
- Progressing towards being formalisied as a RSE Society.

### If you are interested
 Our workshops are available via our GitHub pages

<ul> <span style="font-size: 1em; color: gray; display: block; margin-top: 1em;">
<li> <a href="https://cloud-native-sig.github.io/stfcfeb26-intro-to-kubernetes/">Deploying a Webservice with Kubernetes</a> </li>

<li> <a href="https://github.com/cloud-native-sig/hpcdays26-pocket-sized-kubernetes"> Pocket sized Kubernetes:</a> For this one, to recreate with your own hardware, see the <a href="https://cloud-native-sig.github.io/hpcdays26-pocket-sized-kubernetes/extra-reading">extra reading section</a>.</li>

</span>
</ul>

---

<!-- _class: Title -->
<!-- _header: "" -->
# Introduction to Kubernetes

---

## What is Kubernetes (k8s)

Kubernetes is an open-source container orchestration platform to automate the deployment, scaling & management of containerised applications across groups of machines.

---

# Kubernetes vs Docker Compose

- **Docker:** run apps from built images in containers (sandboxed environments)
- **Docker Compose:** groups of containers with networking and storage on a single host
- **Kubernetes:** Orchestration pipeline for multi-host, multi-container applications.

<small>K8s allows complex containerised applications to run across multiple hosts in a cluster with powerful automation and management features.</small>

---

# Key Components

- **Node**: Physical or virtual machine
- **Control Plane**: Brains of cluster, acts based on current cluster state vs target state
- **Worker**: Run containerised applications using a container runtime.
- **Pod**: Smallest deployable unit&mdash;one or more containers sharing storage/network
- **Deployment**: Target state of identical pods serving an app
- **Service**: Network endpoint for pods

---

# Architecture Overview

<figure style="text-align: center; margin-top:-10px; margin-bottom:-30px; padding-top:0px;">
  <img src="./slide-assets/kubernetes-overview.png" style="height: 500px" alt="The components of a Kubernetes Cluster">
  <figcaption><small>Components of a Kubernetes cluster (<a href="https://kubernetes.io">kubernetes.io</a>)</small></figcaption>
</figure>

<!-- ---

# Kubernetes Distributions

- Vanilla k8s, i.e., [kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/) (CNCF)&mdash;industry standard, fully-featured
- [RKE2](https://docs.rke2.io/) (Rancher Kubernetes Engine 2)&mdash;enterprise, security & compliance
- [K3s](https://k3s.io/) (Rancher)&mdash;Minimal resources & installation, e.g., IoT / Edge Computing
- [MicroK8s](https://canonical.com/microk8s) (Canonical)&mdash;Batteries included, lightweight
- [Minikube](https://minikube.sigs.k8s.io/docs/)&mdash;Single-node cluster for local development -->

---

# How Kubernetes Works

## Benefits

- **Declarative** Configuration. **GitOps:** Infrastructure as code, workflow managers, e.g., [Flux](https://fluxcd.io/), [ArgoCD](https://argoproj.github.io/cd/)
- General **resource management** including networking and security
- Ability to **scale clusters** to include additional cloud resources.
- Offer High Availability (HA) with **Self-Healing**.
<br>

> Example: On *node failure* Kubernetes distributes workload to other nodes

---

# How Kubernetes Works

## Difficulties

- Investing in an entirely new infrastructure
  - Research and Investigation
  - Migration
  - Upskilling

- Fast paced update cycle and dependency chains

---

# Kubernetes uses in Research Computing

Use if you need to manage multiple containerised services across different machines, or for:

- Automatic workload scaling based on demand
- Rolling updates with minimal downtime
- Self-healing resilient systems with high-availability
- Portability between on-prem and cloud infrastructure

---

# Kubernetes not uses in Research Computing

Adds complexity and not typically suitable for:

- Simple container applications on a single host (use Docker Compose)
- HPC job management (use SLURM)
- Large-scale parallel filesystems (e.g., Lustre)
- Services offered by a cloud provider or technology you are already invested in
---

<!-- _class: Title -->
<!-- _header: "" -->
# Use cases

---

<!-- _class: Title -->
<!-- _header: "" -->
<!--# Use case 1: Lab in the Cloud-->


<!-- _class: Title -->
<!-- _header: "" -->
<!--# Use case 2: Jupyter Notebooks-->


<!-- _class: Title -->
<!-- _header: "" -->
<!--# Use case 3: Argo CD and Workflows-->


<!-- _class: Title -->
<!-- _header: "" -->
# Wrap-up

---

# Cloud Native SIG

This webinar was brought to you by the Cloud-Native SIG and CAKE, with support from the Software Sustainability Institute
### You can join the CNSIG

<ul> <span style="font-size: 1em; color: gray; display: block; margin-top: 1em;">
<li> We are looking to grow the SIG so please join our Jisc mailing list for future updates.</li>
<ul>
<li> ✉️  <a href="https://www.jiscmail.ac.uk/cgi-bin/wa-jisc.exe?SUBED1=CLOUDNATIVE-SIG">cloudnative-sig@jiscmail.ac.uk</a> </li>

<li>🌐 <a href="https://cloudnative-sig.ac.uk">cloudnative-sig.ac.uk</a></li>
</ul>
<li>Or if you are deeply interested Kubernetes and Cloud-Native technologies join our committee. Contact any of us on email or LinkedIn to show interest </li>

</span>
</ul>

---

# Next up

**Join the CNSIG at RSECon 2026!** 

We will be running the *Deploying a Webservice with Kubernetes* workshop on Thursday the 10th September. If you're going to be at the conference feel free to come along, and catch the CNSIG throughout the event.

# Stay in touch

Feel free to contact the SIG after the webinar using the websites "Contact Us" page. [https://cloudnative-sig.ac.uk/pages/contact-us.html](https://cloudnative-sig.ac.uk/pages/contact-us.html)

---

<!-- _class: Title -->
<!-- _header: "" -->
# Thank you for listening, any questions?
