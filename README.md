# iOS Shortcut GitHub Action Dispatcher

This repository contains a GitHub Action that can be triggered from an iOS Shortcut to run automated workflows.

## How It Works

The workflow uses GitHub's `repository_dispatch` event, which can be triggered via the GitHub API using a Personal Access Token (PAT).

## Setup Instructions

### 1. Create a Personal Access Token (PAT)

1. Go to your GitHub Settings → Developer Settings → Personal access tokens
2. Create a new token with the `repo` scope
3. Save this token securely - you'll need it for your iOS Shortcut

### 2. Configure the iOS Shortcut

Create a new iOS Shortcut with the following steps:

1. Add a "Text" action to create your payload (customize as needed):
```
{
  "event_type": "ios-shortcut-trigger",
  "client_payload": {
    "task": "Your task description",
    "action": "backup|deploy|report",
    "priority": "high|medium|low",
    "notify": "true|false",
    "request_id": "optional-custom-id"
  }
}
```

2. Add a "URL" action with:
   - URL: `https://api.github.com/repos/YOUR-ORG/actions/dispatches`
   - Method: POST

3. Add a "Get Contents of URL" action with:
   - Headers:
     - Accept: application/vnd.github.v3+json
     - Authorization: token YOUR_PAT_HERE
     - Content-Type: application/json
   - Request Body: Contents of Text (from step 1)
   - Method: POST

### 3. Customize the Action

Edit the `.github/workflows/ios-shortcut-dispatch.yml` file to:
- Add specific actions you want to perform
- Configure notification methods
- Add organizational secrets for authentication

## Security Considerations

- Store your GitHub PAT securely
- Consider using GitHub Organization secrets to manage tokens and other sensitive data
- Limit the scope of your PAT to only what's necessary
- Consider implementing additional validation in the workflow