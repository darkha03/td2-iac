# td2-iac

Infrastructure-as-code project for provisioning an OpenStack environment,
configuring a K3s cluster, and deploying the application components to
Kubernetes with Helm.

## Project structure

```text
td2/
├── ansible/
│   ├── ansible.cfg
│   ├── playbook.yml
│   ├── tunnel.sh
│   └── roles/
│       ├── k3s/
│       │   └── tasks/
│       │       └── main.yml
│       ├── k3s-prereq/
│       │   └── tasks/
│       │       └── main.yml
│       └── kubeconfig/
│           └── tasks/
│               └── main.yml
├── helm/
│   ├── backend/
│   │   ├── .helmignore
│   │   ├── Chart.yaml
│   │   ├── values.yaml
│   │   └── templates/
│   │       ├── configmap-code.yaml
│   │       ├── configmap-code.yaml.bak
│   │       ├── deployment.yaml
│   │       └── service.yaml
│   ├── frontend/
│   │   ├── .helmignore
│   │   ├── Chart.yaml
│   │   ├── values.yaml
│   │   └── templates/
│   │       ├── configmap-html.yaml
│   │       ├── configmap-html.yaml.bak
│   │       ├── configmap-nginx.yaml
│   │       ├── deployment.yaml
│   │       ├── ingress.yaml
│   │       └── service.yaml
│   └── postgres/
│       ├── .helmignore
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
│           ├── configmap-init.yaml
│           ├── service.yaml
│           └── statefulset.yaml
├── terraform/
│   ├── cloud-init.yaml
│   ├── compute.tf
│   ├── data.tf
│   ├── outputs.tf
│   ├── providers.tf
│   ├── security.tf
│   ├── variables.tf
│   └── .terraform.lock.hcl
└── .gitignore
```

## Directory overview

- **`terraform/`** provisions the OpenStack infrastructure and defines
  providers, compute resources, networking data, security rules, variables,
  outputs, and cloud-init configuration.
- **`ansible/`** configures the provisioned machines and installs K3s. The
  playbook applies the prerequisite, K3s, and kubeconfig roles in order.
- **`helm/`** contains the Kubernetes application charts:
  - `frontend` deploys the frontend, Nginx configuration, service, and ingress.
  - `backend` deploys the backend configuration, deployment, and service.
  - `postgres` deploys PostgreSQL configuration, service, and stateful storage.

## Local and generated files

The following environment-specific files may exist locally but are not part of
the version-controlled tree:

```text
ansible/
├── inventory.ini
└── kubeconfig

terraform/
├── terraform.tfvars
├── terraform.tfstate
└── terraform.tfstate.backup
```

The Terraform provider directory (`terraform/.terraform/`) is also generated
locally and ignored by Git. These files can contain environment-specific or
sensitive data; do not commit credentials or private infrastructure
configuration.
