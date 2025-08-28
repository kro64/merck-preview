# ArgoCD Project

GitOps configuration repository for managing applications across different environments using ArgoCD.

## Project Structure

```
├── apps
│   └── webserver
│       ├── preview.yaml    # MR preview deployments (ApplicationSet)
│       ├── prod.yaml       # Production application
│       └── test.yaml       # Test environment application
├── README.md
└── values
    ├── prod
    │   └── k8s-main
    │       └── webserver
    │           └── values.yml    # Production values
    └── test
        └── k8s-main
            └── webserver
                └── values.yml    # Test environment values
```

## Adding New Applications

### 1. Create Application Manifests

Add your application directory under `apps/{app-name}/`:

- **`preview.yaml`**: ApplicationSet for MR preview deployments using merge request generator
- **`prod.yaml`**: Production Application manifest
- **`test.yaml`**: Test environment Application manifest

### 2. Add Environment-Specific Values

Create configuration directories under `values/{environment}/k8s-main/{app-name}/`:

- **`values.yml`**: Helm values for the specific environment
- Include image repositories, resource limits, environment variables, ingress settings, etc.

### Example Structure for New App

```
apps/
└── my-new-app/
    ├── preview.yaml
    ├── prod.yaml
    └── test.yaml

values/
├── prod/
│   └── k8s-main/
│       └── my-new-app/
│           └── values.yml
└── test/
    └── k8s-main/
        └── my-new-app/
            └── values.yml
```

## Key Features

- **Environment Separation**: Clear isolation between test and production configurations
- **MR Preview Deployments**: Automated preview environments for merge requests
- **GitOps Workflow**: All changes tracked in Git with automatic synchronization
- **Centralized Configuration**: Environment-specific values managed in dedicated directories
- **Scalable Architecture**: Consistent pattern for adding new applications

## Workflow Integration

- **Test Environment**: Automatically syncs with latest changes for development testing
- **MR Previews**: ApplicationSet creates temporary deployments for each merge request
- **Production**: Controlled releases triggered by ArgoCD Image Updater detecting new tags