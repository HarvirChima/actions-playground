# Creating an Organization or Enterprise-Level Actions Marketplace

This guide explains how to create an internal actions marketplace for your organization or enterprise, allowing teams to share reusable GitHub Actions across repositories.

## Overview

While GitHub Marketplace is public, you can create a private internal marketplace for your organization or enterprise using several approaches:

| Approach | Scope | Complexity | Best For |
|----------|-------|------------|----------|
| [Internal Repositories](#1-internal-action-repositories) | Org/Enterprise | Low | Simple action sharing |
| [Reusable Workflows](#2-reusable-workflows) | Org/Enterprise | Low | Sharing entire workflows |
| [Private Actions in Internal Repos](#3-private-actions-in-internal-repositories) | Org/Enterprise | Medium | Complex action libraries |
| [Actions Catalog Repository](#4-actions-catalog-repository) | Org/Enterprise | Medium | Centralized discovery |
| [GitHub Packages](#5-using-github-packages) | Org/Enterprise | High | Container-based actions |

---

## 1. Internal Action Repositories

The simplest approach is to create internal repositories that contain your custom actions.

### Setup

1. **Create an internal repository** (e.g., `org-actions` or `shared-actions`)
2. **Organize actions in subdirectories**:

```
org-actions/
├── deploy-to-kubernetes/
│   ├── action.yml
│   └── scripts/
├── security-scan/
│   ├── action.yml
│   └── Dockerfile
├── notify-slack/
│   └── action.yml
└── README.md
```

3. **Use semantic versioning with tags**:

```bash
git tag -a v1.0.0 -m "Release v1.0.0"
git push origin v1.0.0
```

### Usage

Reference the action from any repository in your organization:

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Kubernetes
        uses: my-org/org-actions/deploy-to-kubernetes@v1.0.0
        with:
          cluster: production
          namespace: app
```

### Best Practices

- **Pin to specific versions**: Always use tags (`@v1.0.0`) instead of branches
- **Document each action**: Include README.md in each action directory
- **Use semantic versioning**: Follow semver for releases
- **Create release notes**: Document changes between versions

---

## 2. Reusable Workflows

Share entire workflows across your organization using reusable workflows.

### Setup

1. **Create a workflow with `workflow_call` trigger**:

```yaml
# .github/workflows/standard-ci.yml in org-workflows repo
name: Standard CI Pipeline

on:
  workflow_call:
    inputs:
      node-version:
        description: 'Node.js version'
        required: false
        default: '20'
        type: string
      run-e2e:
        description: 'Run E2E tests'
        required: false
        default: true
        type: boolean
    secrets:
      NPM_TOKEN:
        required: false
      SONAR_TOKEN:
        required: true

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ inputs.node-version }}
      - run: npm ci
      - run: npm run lint

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ inputs.node-version }}
      - run: npm ci
      - run: npm test

  security-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run security scan
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
        run: echo "Running security scan..."
```

2. **Call the reusable workflow from any repository**:

```yaml
# .github/workflows/ci.yml in application repo
name: CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  ci:
    uses: my-org/org-workflows/.github/workflows/standard-ci.yml@v1.0.0
    with:
      node-version: '20'
      run-e2e: true
    secrets:
      SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

### Enterprise Considerations

For enterprise, you can share workflows across organizations:

```yaml
jobs:
  deploy:
    uses: enterprise-org/shared-workflows/.github/workflows/deploy.yml@main
    secrets: inherit  # Pass all secrets automatically
```

---

## 3. Private Actions in Internal Repositories

For organizations on GitHub Enterprise Cloud or Enterprise Server, you can use internal repositories for private action sharing.

### Repository Visibility Settings

1. Go to your organization's **Settings**
2. Navigate to **Actions** → **General**
3. Under "Action permissions", configure which repositories can use internal actions

### Creating an Internal Actions Repository

```yaml
# action.yml for a private composite action
name: 'Internal Deploy Action'
description: 'Deploy applications using internal tools'
inputs:
  environment:
    description: 'Target environment'
    required: true
  version:
    description: 'Version to deploy'
    required: true
outputs:
  deployment-url:
    description: 'URL of the deployed application'
    value: ${{ steps.deploy.outputs.url }}
runs:
  using: 'composite'
  steps:
    - name: Configure internal tools
      shell: bash
      run: |
        echo "Configuring internal deployment tools..."
        
    - name: Deploy application
      id: deploy
      shell: bash
      run: |
        echo "Deploying version ${{ inputs.version }} to ${{ inputs.environment }}"
        echo "url=https://${{ inputs.environment }}.internal.company.com" >> $GITHUB_OUTPUT
```

---

## 4. Actions Catalog Repository

Create a central catalog repository that serves as your internal marketplace documentation.

### Structure

```
actions-catalog/
├── README.md              # Main catalog with index of all actions
├── categories/
│   ├── deployment.md      # Actions for deployment
│   ├── security.md        # Security scanning actions
│   ├── testing.md         # Testing and QA actions
│   └── notifications.md   # Notification actions
├── templates/
│   ├── action-template/   # Template for new actions
│   └── workflow-template/ # Template for new workflows
└── contribution-guide.md  # How to add new actions
```

### Example Catalog README

```markdown
# Internal Actions Marketplace

## 🚀 Deployment

| Action | Description | Version | Maintainer |
|--------|-------------|---------|------------|
| [deploy-kubernetes](https://github.com/org/actions/tree/main/deploy-kubernetes) | Deploy to K8s clusters | v2.1.0 | Platform Team |
| [deploy-serverless](https://github.com/org/actions/tree/main/deploy-serverless) | AWS Lambda deployments | v1.3.0 | Cloud Team |

## 🔐 Security

| Action | Description | Version | Maintainer |
|--------|-------------|---------|------------|
| [security-scan](https://github.com/org/actions/tree/main/security-scan) | SAST/DAST scanning | v3.0.0 | Security Team |
| [dependency-check](https://github.com/org/actions/tree/main/dependency-check) | Dependency vulnerability scan | v1.2.0 | Security Team |

## 🧪 Testing

| Action | Description | Version | Maintainer |
|--------|-------------|---------|------------|
| [e2e-tests](https://github.com/org/actions/tree/main/e2e-tests) | End-to-end testing | v2.0.0 | QA Team |
| [load-test](https://github.com/org/actions/tree/main/load-test) | Performance testing | v1.1.0 | Performance Team |
```

---

## 5. Using GitHub Packages

For more complex actions, you can distribute them as container images via GitHub Packages.

### Creating a Docker Action

```dockerfile
# Dockerfile
FROM node:20-alpine

COPY entrypoint.sh /entrypoint.sh
COPY lib/ /lib/

RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
```

```yaml
# action.yml
name: 'Container-based Action'
description: 'Action distributed via GitHub Container Registry'
inputs:
  config:
    description: 'Configuration file path'
    required: true
runs:
  using: 'docker'
  image: 'docker://ghcr.io/my-org/my-action:v1.0.0'
  args:
    - ${{ inputs.config }}
```

### Publishing to GitHub Container Registry

```bash
# Build the image
docker build -t ghcr.io/my-org/my-action:v1.0.0 .

# Authenticate with GHCR
echo $GITHUB_TOKEN | docker login ghcr.io -u USERNAME --password-stdin

# Push the image
docker push ghcr.io/my-org/my-action:v1.0.0
```

---

## Enterprise Features

### GitHub Enterprise Cloud

- **Internal repositories**: Share actions with all organization members
- **IP allow lists**: Control access to actions from specific networks
- **Audit logs**: Track action usage across the enterprise
- **Enterprise-wide policies**: Set action usage policies across all organizations

### GitHub Enterprise Server

- **GitHub Connect**: Sync actions from GitHub.com
- **Local Actions**: Host actions on your GHES instance
- **Network isolation**: Keep all action code within your network
- **Custom action runner groups**: Control where actions execute

### Configuring Enterprise Policies

1. Navigate to **Enterprise settings** → **Policies** → **Actions**
2. Configure:
   - Which actions are allowed (all, local only, select actions)
   - Repository access to actions
   - Required workflows for repositories

---

## Governance and Best Practices

### Action Versioning Strategy

```
v1.0.0  - Major: Breaking changes
v1.1.0  - Minor: New features, backward compatible
v1.1.1  - Patch: Bug fixes only
```

### Security Considerations

1. **Code review**: Require reviews for all action changes
2. **Signed commits**: Enable commit signing for action repositories
3. **Dependency scanning**: Enable Dependabot for action dependencies
4. **Secret scanning**: Enable secret scanning on action repositories
5. **CODEOWNERS**: Define owners for action files

```
# CODEOWNERS file
*.yml @org/platform-team
action.yml @org/platform-team @org/security-team
```

### Documentation Requirements

Each action should include:

- **README.md**: Description, inputs, outputs, examples
- **CHANGELOG.md**: Version history and changes
- **action.yml**: Well-documented inputs/outputs
- **examples/**: Example workflows using the action

### Testing Actions

```yaml
# .github/workflows/test-action.yml
name: Test Action

on:
  pull_request:
    paths:
      - 'my-action/**'
      - '.github/workflows/test-action.yml'

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Test the action
        id: test
        uses: ./my-action
        with:
          input1: test-value
          
      - name: Verify outputs
        run: |
          if [[ "${{ steps.test.outputs.result }}" != "expected" ]]; then
            echo "Action output does not match expected value"
            exit 1
          fi
```

---

## Migrating to Internal Marketplace

### Step-by-Step Migration

1. **Audit existing workflows**: Find commonly duplicated patterns
2. **Identify candidates**: Look for steps used across multiple repositories
3. **Create action repository**: Set up your internal actions repository
4. **Extract and refactor**: Convert duplicated steps into reusable actions
5. **Document and publish**: Add documentation and version tags
6. **Communicate**: Announce new actions to teams
7. **Update workflows**: Migrate repositories to use shared actions
8. **Monitor and maintain**: Track usage and keep actions updated

### Example: Before and After

**Before** (duplicated in many repositories):

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Configure AWS
        run: |
          aws configure set aws_access_key_id ${{ secrets.AWS_KEY }}
          aws configure set aws_secret_access_key ${{ secrets.AWS_SECRET }}
      - name: Deploy to ECS
        run: |
          aws ecs update-service --cluster prod --service app --force-new-deployment
```

**After** (using internal action):

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: my-org/org-actions/deploy-ecs@v1.0.0
        with:
          cluster: prod
          service: app
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_KEY }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET }}
```

---

## Additional Resources

- [GitHub Docs: Sharing actions in your organization](https://docs.github.com/en/actions/creating-actions/sharing-actions-and-workflows-with-your-organization)
- [GitHub Docs: Reusing workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows)
- [GitHub Docs: About internal repositories](https://docs.github.com/en/repositories/creating-and-managing-repositories/about-repositories#about-internal-repositories)
- [GitHub Enterprise: Managing policies](https://docs.github.com/en/enterprise-cloud@latest/admin/policies/enforcing-policies-for-your-enterprise/enforcing-policies-for-github-actions-in-your-enterprise)

---

## Summary

Creating an internal actions marketplace involves:

1. **Organization level**: Use internal repositories with composite actions and reusable workflows
2. **Enterprise level**: Configure enterprise policies, use GitHub Connect (for GHES), and create organization-wide standards
3. **Discovery**: Create a catalog repository with documentation of available actions
4. **Governance**: Implement versioning, security scanning, code review, and documentation standards

The key is to start simple with a few high-value actions, document them well, and expand based on team feedback and adoption.
