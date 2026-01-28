# Example: How to Trigger repository_dispatch

The `repository_dispatch` event allows you to trigger a workflow from outside GitHub, such as from an external webhook or API call.

## Prerequisites

You need a GitHub Personal Access Token (PAT) with `repo` scope:
1. Go to GitHub Settings → Developer settings → Personal access tokens
2. Click "Generate new token (classic)"
3. Select the `repo` scope
4. Generate and copy the token

## Using curl

```bash
curl -X POST \
  -H "Accept: application/vnd.github.v3+json" \
  -H "Authorization: token YOUR_GITHUB_TOKEN" \
  https://api.github.com/repos/HarvirChima/actions-playground/dispatches \
  -d '{
    "event_type": "custom-event",
    "client_payload": {
      "message": "Hello from API",
      "timestamp": "2024-01-01T00:00:00Z",
      "user": "demo-user"
    }
  }'
```

## Using GitHub CLI

First, install [GitHub CLI](https://cli.github.com/) if you haven't already.

```bash
# Trigger with custom-event type
# Note: On macOS, use: date -u +%Y-%m-%dT%H:%M:%SZ (BSD date)
# On Linux: date -u +%Y-%m-%dT%H:%M:%SZ (GNU date)
gh api repos/HarvirChima/actions-playground/dispatches \
  -F event_type=custom-event \
  -F client_payload[message]="Hello from GitHub CLI" \
  -F client_payload[timestamp]="$(date -u +%Y-%m-%dT%H:%M:%SZ)"

# Trigger with webhook-event type
gh api repos/HarvirChima/actions-playground/dispatches \
  -F event_type=webhook-event \
  -F client_payload[source]="external-service" \
  -F client_payload[data]="Important webhook data"
```

## Using Node.js/JavaScript

```javascript
// Using built-in fetch (Node.js 18+)
// For older Node.js versions, use node-fetch v2: npm install node-fetch@2

const token = 'YOUR_GITHUB_TOKEN';
const owner = 'HarvirChima';
const repo = 'actions-playground';

async function triggerWorkflow() {
  const response = await fetch(
    `https://api.github.com/repos/${owner}/${repo}/dispatches`,
    {
      method: 'POST',
      headers: {
        'Accept': 'application/vnd.github.v3+json',
        'Authorization': `token ${token}`,
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        event_type: 'custom-event',
        client_payload: {
          message: 'Hello from Node.js',
          timestamp: new Date().toISOString(),
        },
      }),
    }
  );

  if (response.ok) {
    console.log('Workflow triggered successfully!');
  } else {
    console.error('Failed to trigger workflow:', await response.text());
  }
}

triggerWorkflow();
```

## Using Python

```python
import requests
from datetime import datetime

token = 'YOUR_GITHUB_TOKEN'
owner = 'HarvirChima'
repo = 'actions-playground'

def trigger_workflow():
    url = f'https://api.github.com/repos/{owner}/{repo}/dispatches'
    headers = {
        'Accept': 'application/vnd.github.v3+json',
        'Authorization': f'token {token}',
    }
    data = {
        'event_type': 'custom-event',
        'client_payload': {
            'message': 'Hello from Python',
            'timestamp': datetime.utcnow().isoformat() + 'Z',
        }
    }
    
    response = requests.post(url, headers=headers, json=data)
    
    if response.status_code == 204:
        print('Workflow triggered successfully!')
    else:
        print(f'Failed to trigger workflow: {response.status_code}')
        print(response.text)

trigger_workflow()
```

## Event Types

The `04-repository-dispatch.yml` workflow listens for these event types:
- `custom-event`: For custom application events
- `webhook-event`: For external webhook notifications

You can add more event types by modifying the workflow's `types` array.

## Client Payload

The `client_payload` is a JSON object that you can use to pass custom data to the workflow. This data is accessible in the workflow via `${{ github.event.client_payload }}`.

Example payloads:

### Deployment trigger
```json
{
  "event_type": "custom-event",
  "client_payload": {
    "environment": "production",
    "version": "v1.2.3",
    "triggered_by": "deployment-system"
  }
}
```

### Build trigger
```json
{
  "event_type": "custom-event",
  "client_payload": {
    "build_type": "release",
    "platform": "linux",
    "architecture": "x64"
  }
}
```

## Viewing Results

After triggering the workflow:
1. Go to the **Actions** tab in the repository
2. Look for "04 - Repository Dispatch" workflow
3. You should see a new workflow run
4. Click on it to see the logs and the payload you sent

## Use Cases

Repository dispatch is useful for:
- Triggering deployments from external CI/CD systems
- Responding to webhooks from external services
- Integrating with custom automation systems
- Cross-repository workflow triggers
- Event-driven automation

## Security Notes

⚠️ **Important Security Considerations:**
- Never commit your Personal Access Token to a repository
- Use environment variables or secrets management for tokens
- Rotate tokens regularly
- Use the principle of least privilege (only grant necessary scopes)
- Consider using GitHub Apps for production systems (more secure than PATs)
- Validate and sanitize any client_payload data in your workflows

## Troubleshooting

### 404 Not Found
- Check that the repository name and owner are correct
- Verify your token has the `repo` scope

### 401 Unauthorized
- Verify your token is valid and not expired
- Check that the token has the `repo` scope

### No workflow triggered
- Ensure the `event_type` matches one defined in the workflow's `types` array
- Check that the workflow file is on the default branch
- Verify the workflow is not disabled

### Rate Limiting
- GitHub API has rate limits (5000 requests/hour for authenticated requests)
- If you hit limits, wait or use multiple tokens
- Check remaining limits: `gh api rate_limit`