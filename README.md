# GitHub Actions Playground 🚀

A comprehensive demo repository showcasing various GitHub Actions concepts, patterns, and best practices. Perfect for learning, teaching, or demonstrating GitHub Actions capabilities.

## 📚 Table of Contents

- [Overview](#overview)
- [Workflow Examples](#workflow-examples)
- [Quick Start](#quick-start)
- [Topics Covered](#topics-covered)
- [How to Use This Repository](#how-to-use-this-repository)
- [Additional Resources](#additional-resources)

## Overview

This repository contains working examples of GitHub Actions workflows covering everything from basic concepts to advanced patterns. Each workflow is well-documented with inline comments explaining the concepts being demonstrated.

## Workflow Examples

### Basic Concepts

| Workflow | Description | Key Concepts |
|----------|-------------|--------------|
| [01 - Basic Workflow](.github/workflows/01-basic-workflow.yml) | Introduction to workflows | Push/PR triggers, jobs, steps, checkout action |
| [02 - Manual Workflow Dispatch](.github/workflows/02-workflow-dispatch.yml) | Manual triggering with inputs | workflow_dispatch, input types (choice, boolean, string) |
| [03 - Scheduled Workflow](.github/workflows/03-scheduled-workflow.yml) | Scheduled/cron jobs | Cron syntax, scheduled triggers |
| [04 - Repository Dispatch](.github/workflows/04-repository-dispatch.yml) | External event triggers | repository_dispatch, API webhooks |
| [12 - Advanced Triggers](.github/workflows/12-advanced-triggers.yml) | Complex trigger configurations | Path filters, branch filters, activity types |

### Jobs and Runners

| Workflow | Description | Key Concepts |
|----------|-------------|--------------|
| [05 - Jobs and Runners](.github/workflows/05-jobs-and-runners.yml) | Multiple jobs and platforms | Ubuntu/Windows/macOS runners, job dependencies, matrix strategy |

### Marketplace and Actions

| Workflow | Description | Key Concepts |
|----------|-------------|--------------|
| [06 - Marketplace Actions](.github/workflows/06-marketplace-actions.yml) | Using community actions | setup-node, setup-python, artifacts, verified creators |

### Security and Secrets

| Workflow | Description | Key Concepts |
|----------|-------------|--------------|
| [07 - Secrets](.github/workflows/07-secrets.yml) | Working with secrets | Repository secrets, environments, secret masking |

### Performance and Optimization

| Workflow | Description | Key Concepts |
|----------|-------------|--------------|
| [08 - Caching](.github/workflows/08-caching.yml) | Dependency and build caching | Cache action, cache keys, restore keys |

### Reusability

| Workflow | Description | Key Concepts |
|----------|-------------|--------------|
| [09 - Reusable Workflows](.github/workflows/09-reusable-workflow.yml) | Calling reusable workflows | workflow_call, inputs, outputs, DRY principle |
| [Reusable Build Workflow](.github/workflows/reusable-build.yml) | Reusable workflow definition | workflow_call trigger, parameters, secrets passing |
| [10 - Composite Actions](.github/workflows/10-composite-action.yml) | Using composite actions | Custom actions, composite actions, action inputs/outputs |
| [Greet User Action](.github/actions/greet-user/action.yml) | Composite action definition | Composite action structure, shell steps |

### Best Practices

| Workflow | Description | Key Concepts |
|----------|-------------|--------------|
| [11 - Best Practices](.github/workflows/11-best-practices.yml) | Patterns and recommendations | Security, performance, naming, error handling |

## Quick Start

### Running Workflows

1. **Automatic Triggers**: Push to the `main` or `copilot/**` branches to trigger basic workflows
2. **Manual Triggers**: Go to **Actions** tab → Select a workflow with "workflow_dispatch" → Click **Run workflow**
3. **Scheduled**: Scheduled workflows run automatically (or can be triggered manually for testing)

### Viewing Results

1. Navigate to the **Actions** tab in this repository
2. Select a workflow from the left sidebar
3. Click on a specific workflow run to see details
4. Expand jobs and steps to see logs and outputs

## Topics Covered

### ✅ Core Concepts
- Workflows and pipelines
- Jobs, steps, and runners
- Events and triggers
- GitHub Actions syntax

### ✅ Events and Triggers
- **Basic**: `push`, `pull_request`
- **Advanced**: `workflow_dispatch`, `schedule`, `repository_dispatch`
- **Filters**: Branch filters, path filters, tag filters
- **Activity Types**: PR events, issue events, comment events

### ✅ Jobs and Runners
- Multiple jobs (parallel and sequential)
- Job dependencies
- Different runner types (Ubuntu, Windows, macOS)
- Matrix strategy for parallel testing
- Self-hosted runners concepts

### ✅ Marketplace Actions
- Using actions from GitHub Marketplace
- Verified creators
- Setup actions (Node.js, Python, Go)
- Artifact upload/download
- Popular community actions

### ✅ Secrets Management
- Repository secrets
- Environment-specific secrets
- Secret masking in logs
- Security best practices

### ✅ Caching
- Dependency caching (npm, pip)
- Build output caching
- Cache key strategies
- Restore keys

### ✅ Reusability
- **Reusable Workflows**: Share entire workflows across repositories
- **Composite Actions**: Package multiple steps into reusable actions
- Input parameters and outputs
- Best practices for modular workflows

### ✅ Best Practices
- Security considerations
- Performance optimization
- Error handling
- Naming conventions
- Conditional execution
- Environment variables

## How to Use This Repository

### For Learning
1. Start with [01-basic-workflow.yml](.github/workflows/01-basic-workflow.yml)
2. Progress through numbered workflows sequentially
3. Read inline comments for detailed explanations
4. Trigger workflows manually to see them in action

### For Demos
1. Clone or fork this repository
2. Use the **Actions** tab to demonstrate live workflow execution
3. Show manual triggers with different inputs
4. Highlight specific concepts using the workflows as examples
5. Refer to this README for quick navigation

### For Reference
- Use workflows as templates for your own projects
- Copy and adapt patterns to your needs
- Reference the inline documentation
- Explore different trigger types and configurations

## Additional Resources

### GitHub Documentation
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
- [Events that trigger workflows](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)
- [GitHub Actions Marketplace](https://github.com/marketplace?type=actions)

### Best Practices
- [Security hardening](https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions)
- [Usage limits](https://docs.github.com/en/actions/learn-github-actions/usage-limits-billing-and-administration)
- [Caching dependencies](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows)

## Repository Structure

```
.
├── .github/
│   ├── workflows/           # All workflow examples
│   │   ├── 01-basic-workflow.yml
│   │   ├── 02-workflow-dispatch.yml
│   │   ├── 03-scheduled-workflow.yml
│   │   ├── 04-repository-dispatch.yml
│   │   ├── 05-jobs-and-runners.yml
│   │   ├── 06-marketplace-actions.yml
│   │   ├── 07-secrets.yml
│   │   ├── 08-caching.yml
│   │   ├── 09-reusable-workflow.yml
│   │   ├── 10-composite-action.yml
│   │   ├── 11-best-practices.yml
│   │   ├── 12-advanced-triggers.yml
│   │   └── reusable-build.yml
│   └── actions/             # Custom composite actions
│       └── greet-user/
│           └── action.yml
└── README.md               # This file
```

## Contributing

This is a demo repository. Feel free to:
- Fork it for your own demos
- Suggest improvements via issues
- Add more examples via pull requests

## License

This repository is provided as-is for educational and demonstration purposes.

---

**Happy Learning! 🎉**

For questions or suggestions, please open an issue in this repository.
