# Remediator

AI-powered GitHub Action that automatically scans your workflows for security vulnerabilities and creates pull requests with fixes.

## Features

- **Deep Security Analysis** - Scans GitHub workflows using static analysis tools
- **AI-Powered Fixes** - Uses advanced LLMs to generate contextual security fixes  
- **Automated PRs** - Creates pull requests with detailed explanations and fixes
- **Cloud-Powered** - Leverages cloud infrastructure for scalable processing
- **High Performance** - Optimized Go binary with integrated analysis engine
- **Usage Tracking** - API token-based usage limits and analytics

## Quick Start

### 1. Get Your API Token

1. Visit [remediator.ai](https://remediator.ai/)
2. Sign in with GitHub or GitLab OAuth
3. Subscribe to premium via Stripe
4. Copy your API token

### 2. Add Secrets to Your Repository

Go to your repository **Settings** -> **Secrets and variables** -> **Actions** and add:

| Secret Name | Description | Required |
|-------------|-------------|----------|
| `FS_API_TOKEN` | Your premium API token from remediator | Yes |
| `GH_PAT` | GitHub Personal Access Token with repo permissions | Yes |

### 3. Create Workflow

Create `.github/workflows/security-scan.yml`:

```yaml
name: Workflow Security Scan

on:
  pull_request:
    branches: [ main ]
  workflow_dispatch:

jobs:
  security-scan:
    if: ${{ !contains(github.event.pull_request.title, 'Security Audit & Fixes for GitHub Actions Workflows') }}
    runs-on: ubuntu-latest
    permissions:
      contents: write
      pull-requests: write
      issues: write
    
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        
      - name: Run workflow scanner
        uses: Scalabit/workflow-scanner-action@master
        with:
          api-token: ${{ secrets.FS_API_TOKEN }}
          github-token: ${{ secrets.GH_PAT }}
          provider-api-key: ${{ secrets.PROVIDER_API_KEY }} # change provider to either openai/gemini/anthropic
          target-branch: main
```

## Input Parameters

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `api-token` | API token from remediator service | Yes | - |
| `github-token` | GitHub token with repo permissions | Yes | `${{ github.token }}` |
| `repository` | Repository in format `owner/repo` | No | `${{ github.repository }}` |
| `target-branch` | Branch to scan in the target repository | No | `main` |

## Advanced Configuration

You can specify LLM provider and model by adding these inputs:

| Input | Description | Example Values | Default on model|
|-------|-------------|----------------| ---------------
| `openai-api-key` | OpenAI API key | `sk-...` | gpt-4o|
| `anthropic-api-key` | Anthropic API key | `sk-ant-...` | claude-3-5-sonnet |
| `gemini-api-key` | Google Gemini API key | `AIza....` | gemini-2.5-flash |
| `model` | Specific model to use | `gpt-4o`, `claude-3-5-sonnet`, `gemini-2.5-pro` | Above

### Example with Custom Model

```yaml
- name: Run workflow scanner
  uses: Scalabit/workflow-scanner-action@main
  with:
    api-token: ${{ secrets.FS_API_TOKEN }}
    github-token: ${{ secrets.GITHUB_TOKEN }}
    target-branch: develop
    openai-api-key: ${{ secrets.OPENAI_API_KEY }}
    model: gpt-4o-mini
```

## Outputs

This action does not produce any outputs. It scans your workflows and creates a pull request if issues are found.

## Troubleshooting

- **Invalid API token**: Check secrets and subscription status
- **Permission errors**: Verify GitHub token has repo access
- **Missing secrets**: Ensure all required secrets are set
- **Docker build issues**: The action builds containers locally, no Docker Hub authentication required

[Get started at remediator.ai](https://remediator.ai/)

#### Further info also on website
---

**Made with your security in mind by [Scalabit](https://scalabit.dev/)**
