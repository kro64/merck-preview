# CICD Templates

Reusable GitLab CI/CD templates for common platform operations.

## Vault Integration

### Quick Start
```yaml
include:
  - project: 'platform/cicd-templates'
    file: '/vault/auth.yml'

get-secrets:
  extends: .vault_get_secrets
  variables:
    VAULT_SECRETS: "k8s/registry:user:REGISTRY_USER k8s/database:password:DB_PASS"

deploy:
  dependencies: [get-secrets]
  script:
    - echo "Using secrets: $REGISTRY_USER, $DB_PASS"
```

### Secret Format
`VAULT_SECRETS: "path:field:ENV_VAR path2:field2:ENV_VAR2"`

- **path**: Vault secret path (e.g., `k8s/registry`)
- **field**: Secret field name (e.g., `user`, `password`)  
- **ENV_VAR**: Environment variable name for the job

## Requirements
- Project must have `k8s-readonly` policy attached to GitLab JWT role
- Vault accessible at `http://vault.vault:8200`
