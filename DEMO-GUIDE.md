# GitHub Actions Demo Guide

This guide provides instructions for demonstrating all the GitHub Actions features in this repository.

## Presentation Order

For a comprehensive demo, present the workflows in this order:

1. **01 - Basic Workflow** - Start with fundamentals
2. **05 - Jobs and Runners** - Show parallelism and different platforms
3. **02 - Manual Workflow Dispatch** - Demonstrate interactive inputs
4. **12 - Advanced Triggers** - Show path filters and advanced triggers
5. **03 - Scheduled Workflow** - Explain cron scheduling
6. **06 - Marketplace Actions** - Show community actions
7. **08 - Caching** - Demonstrate performance optimization
8. **09 - Reusable Workflows** - Show code reuse patterns
9. **10 - Composite Actions** - Demonstrate custom actions
10. **07 - Secrets** - Cover security practices
11. **11 - Best Practices** - Summarize recommendations
12. **04 - Repository Dispatch** - Advanced external triggers

## How to Trigger Each Workflow

### Automatic Triggers

These workflows run automatically on push:
- 01 - Basic Workflow
- 05 - Jobs and Runners
- 06 - Marketplace Actions
- 08 - Caching
- 09 - Reusable Workflows
- 10 - Composite Actions
- 11 - Best Practices
- 12 - Advanced Triggers

**To trigger**: Just push to `main` or `copilot/**` branches, or create a pull request.

### Manual Triggers

These workflows have `workflow_dispatch` and can be triggered manually:

**Steps**:
1. Go to the **Actions** tab
2. Select the workflow from the left sidebar
3. Click **Run workflow** button
4. Fill in any required inputs
5. Click **Run workflow**

Workflows with manual trigger:
- 02 - Manual Workflow Dispatch (demonstrates various input types)
- 03 - Scheduled Workflow (also runs on schedule)
- 07 - Secrets (environment-specific demo)

### Scheduled Trigger

**03 - Scheduled Workflow** runs automatically at 00:00 UTC daily, but can also be triggered manually for immediate demonstration.

### Repository Dispatch

**04 - Repository Dispatch** is triggered via API call. To trigger it:

#### Using curl:
```bash
curl -X POST \
  -H "Accept: application/vnd.github.v3+json" \
  -H "Authorization: token YOUR_GITHUB_TOKEN" \
  https://api.github.com/repos/HarvirChima/actions-playground/dispatches \
  -d '{"event_type":"custom-event","client_payload":{"message":"Hello from API"}}'
```

#### Using GitHub CLI:
```bash
gh api repos/HarvirChima/actions-playground/dispatches \
  -F event_type=custom-event \
  -F client_payload[message]="Hello from API"
```

**Note**: You need a GitHub Personal Access Token with `repo` scope.

## Demo Tips

### For Live Demos

1. **Pre-run workflows**: Before the demo, trigger workflows so you have recent runs to show
2. **Keep tabs open**: Have the Actions tab open in multiple tabs for quick switching
3. **Use workflow_dispatch**: This allows you to trigger workflows on-demand during the demo
4. **Show logs**: Expand steps to show detailed logs and outputs
5. **Highlight inline comments**: Show the workflow YAML files with their inline documentation

### Key Points to Emphasize

#### Basic Concepts
- Workflows are defined in `.github/workflows/`
- YAML syntax for configuration
- Events trigger workflows
- Jobs contain steps
- Steps execute commands or actions

#### Events and Triggers
- Multiple trigger types available
- Can combine multiple triggers
- Filters provide fine-grained control
- Manual triggers great for deployments

#### Jobs and Runners
- Jobs run in parallel by default
- Use `needs` for dependencies
- Multiple OS support
- Matrix strategy for testing multiple configurations

#### Marketplace Actions
- Thousands of pre-built actions available
- Verified creators badge
- Setup actions for different languages
- Community-driven ecosystem

#### Secrets
- Never commit secrets to code
- Use GitHub Secrets for sensitive data
- Automatically masked in logs
- Environment-specific secrets for different stages

#### Caching
- Significantly speeds up workflows
- Cache dependencies between runs
- Use cache keys wisely
- Understand cache limits

#### Reusability
- DRY principle for workflows
- Reusable workflows for entire pipelines
- Composite actions for step sequences
- Share across repositories

#### Best Practices
- Security first
- Pin action versions
- Use meaningful names
- Optimize for performance
- Handle errors appropriately

## Common Questions

### Q: How much do GitHub Actions cost?
**A**: Free for public repositories. For private repos, GitHub provides free minutes based on your plan, then charges for additional usage.

### Q: Can I use my own runners?
**A**: Yes! Self-hosted runners are supported and useful for specific requirements or better performance.

### Q: How do I debug failing workflows?
**A**: 
- Check the workflow logs in the Actions tab
- Use `echo` statements for debugging
- Enable debug logging with secrets `ACTIONS_STEP_DEBUG` and `ACTIONS_RUNNER_DEBUG`
- Test workflows locally with tools like `act`

### Q: Can I run workflows locally?
**A**: Yes, using tools like [act](https://github.com/nektos/act) which runs GitHub Actions locally using Docker.

### Q: What are the usage limits?
**A**:
- Workflow run time: 35 days max
- Job execution time: 6 hours max (72 hours for self-hosted)
- API rate limits apply
- Concurrent jobs limited based on plan

### Q: How do I secure my workflows?
**A**:
- Pin actions to specific commit SHAs
- Use verified creators
- Review third-party actions
- Limit workflow permissions
- Use environments with required reviewers
- Enable Dependabot for actions

## Additional Demo Ideas

### Create a Full CI/CD Pipeline
Combine multiple concepts:
1. Trigger on PR
2. Run linting and tests in parallel
3. Build application
4. Upload artifacts
5. Deploy to staging (on merge to main)
6. Manual approval for production
7. Deploy to production

### Show Real-World Examples
- **Web App Deployment**: Build and deploy to Vercel/Netlify
- **Container Build**: Build and push Docker images
- **Documentation**: Auto-generate and deploy docs
- **Release Automation**: Create releases with changelogs
- **Dependency Updates**: Automated dependency PRs

### Interactive Demonstrations
1. **Modify a workflow live**: Edit a workflow file and show the changes reflected
2. **Trigger with different inputs**: Show how input parameters change behavior
3. **Show failed runs**: Demonstrate error handling and retry logic
4. **Matrix builds**: Show parallel execution across multiple configurations

## Resources for Attendees

Share these resources with your demo audience:
- This repository: https://github.com/HarvirChima/actions-playground
- GitHub Actions Documentation: https://docs.github.com/en/actions
- GitHub Actions Marketplace: https://github.com/marketplace?type=actions
- Awesome Actions: https://github.com/sdras/awesome-actions
- GitHub Actions Community Forum: https://github.community/c/code-to-cloud/github-actions

## Troubleshooting

### Workflows not appearing
- Ensure files are in `.github/workflows/` directory
- Check YAML syntax is valid
- Verify file has `.yml` or `.yaml` extension

### Workflows not triggering
- Check trigger conditions (branches, paths)
- Verify permissions (for repository_dispatch)
- Check if workflow is disabled

### Secrets not working
- Verify secret names match exactly (case-sensitive)
- Ensure secrets are set at appropriate level (repo/environment)
- Remember secrets are not available in forks for security

---

**Ready to Demo!** 🎬

Use this guide as a reference during your GitHub Actions demonstration. Feel free to adapt the order and content based on your audience's needs and interests.