# Workflow Security Scanner

AI-powered GitHub Action that automatically scans your workflows for security vulnerabilities and creates pull requests with fixes.

## Features

- **Deep Security Analysis** - Scans GitHub workflows for 40+ security vulnerabilities
- **AI-Powered Fixes** - Uses advanced LLMs to generate contextual security fixes
- **Automated PRs** - Creates pull requests with detailed explanations and fixes
- **Zero Configuration** - Works out of the box with minimal setup
- **Fast & Scalable** - Cloud-native processing with Google Batch
- **Detailed Reports** - Comprehensive security analysis and recommendations

## Quick Start

### 1. Get Your API Token

1. Visit [Workflow Scanner](https://workflow-scanner-36bg3tpnra-lz.a.run.app)
2. Sign in with GitHub OAuth
3. Subscribe to premium (€10/month)
4. Copy your API token

### 2. Add Secrets to Your Repository

Go to your repository **Settings** -> **Secrets and variables** -> **Actions** and add:

| Secret Name | Description | Required |
|-------------|-------------|----------|
| `WORKFLOW_SCANNER_API_TOKEN` | Your premium API token from the dashboard | Yes |
| `LLM_API_KEY` | OpenAI, Anthropic, or Gemini API key | Yes |

### 3. Create Workflow

Create `.github/workflows/security-scan.yml`:

```yaml
name: Workflow Security Scan

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  security-scan:
    runs-on: ubuntu-latest
    name: Scan workflows for security issues
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        
      - name: Run security scan
        uses: your-org/workflow-scanner-action@v1
        with:
          api-token: ${{ secrets.WORKFLOW_SCANNER_API_TOKEN }}
          github-token: ${{ secrets.GITHUB_TOKEN }}
          llm-api-key: ${{ secrets.LLM_API_KEY }}
```

## Input Parameters

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `api-token` | API token from workflow scanner service | Yes | - |
| `github-token` | GitHub token with repo permissions | Yes | - |
| `llm-api-key` | LLM API key (OpenAI/Anthropic/Gemini) | Yes | - |
| `repository` | Repository in format `owner/repo` | No | `${{ github.repository }}` |
| `service-url` | Service URL (for testing) | No | Production URL |

## Outputs

| Output | Description |
|--------|-------------|
| `job-id` | Batch job ID for tracking scan progress |

## Advanced Usage

### Scan on Pull Requests Only

```yaml
on:
  pull_request:
    types: [opened, synchronize, reopened]
```

### Custom Repository Scanning

```yaml
- name: Scan specific repository
  uses: your-org/workflow-scanner-action@v1
  with:
    api-token: ${{ secrets.WORKFLOW_SCANNER_API_TOKEN }}
    github-token: ${{ secrets.GITHUB_TOKEN }}
    llm-api-key: ${{ secrets.LLM_API_KEY }}
    repository: "other-org/other-repo"
```

### Multiple LLM Providers

The action supports multiple AI providers. Set your `LLM_API_KEY` to any of:

- **OpenAI**: `sk-...` (GPT-4, GPT-3.5)
- **Anthropic**: `sk-ant-...` (Claude)
- **Google**: Gemini API key

## Security & Privacy

- **Private Processing**: Your code is processed in secure, isolated containers
- **No Data Storage**: Source code is not stored
- **Token Security**: All tokens are encrypted and securely transmitted
- **Audit Logs**: All scans are logged for security compliance

## What We Detect

### High-Risk Vulnerabilities
- Hardcoded secrets and API keys
- Unsafe script injection patterns  
- Privilege escalation risks
- Insecure artifact handling

### Medium-Risk Issues
- Missing security headers
- Overprivileged permissions
- Insecure checkout configurations
- Vulnerable dependency patterns

### Best Practice Violations
- Missing workflow validation
- Insecure environment usage
- Poor secret management
- Inadequate access controls

## Example Output

After scanning, the action will:

1. **Analyze** your workflows for security issues
2. **Generate** fixes using AI
3. **Create** a pull request with:
   - Detailed vulnerability explanations
   - Secure code replacements
   - References to security best practices
   - Risk assessment and impact analysis

### Sample Pull Request

```markdown
## Security Fixes for GitHub Workflows

This PR addresses 3 security vulnerabilities found in your workflows:

### High Risk: Hardcoded Secret (workflow.yml:15)
- **Issue**: API key exposed in plain text
- **Fix**: Moved to GitHub Secrets
- **Impact**: Prevents credential exposure

### Medium Risk: Script Injection (build.yml:23)  
- **Issue**: Unsanitized user input in script
- **Fix**: Added input validation and escaping
- **Impact**: Prevents malicious code execution
```

## Troubleshooting

### Common Issues

#### "Invalid or expired API token"
- Verify your token in repository secrets
- Check if your subscription is active
- Regenerate token from the dashboard

#### "Failed to submit scan"
- Check if your GitHub token has necessary permissions
- Verify repository access permissions
- Ensure LLM API key is valid and has credits

#### "Missing required fields"
- Confirm all required secrets are set
- Check secret names match exactly
- Verify no typos in workflow YAML


[Subscribe now](https://workflow-scanner-36bg3tpnra-lz.a.run.app) to get started!

---

**Made with your security in mind by [Scalabit](https://scalabit.dev/)**

*Secure your workflows before they secure you!*