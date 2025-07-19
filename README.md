# Helm Charts Repository

A modern Helm charts repository using **OCI Registry** (GitHub Container Registry) for hosting charts with automated CI/CD.

## 🚀 Features

- **OCI Registry**: Modern approach using GitHub Container Registry (ghcr.io)
- **Automated Testing**: Pull requests trigger linting, unit tests, and integration tests
- **Version Management**: Automatic tagging when chart versions change
- **Release Automation**: Tagged releases automatically push charts to OCI registry
- **GitHub Releases**: Clear release notes with installation instructions
- **Demo Application**: Includes a sample nginx-based application chart

## 📦 Quick Start

### Install Charts Directly (Recommended)
```bash
# Install the demo-app chart
helm install my-demo oci://ghcr.io/sudoaaron/helm-charts/demo-app --version 0.2.2

# Install with custom values
helm install my-demo oci://ghcr.io/sudoaaron/helm-charts/demo-app --version 0.2.2 \
  --set replicaCount=3 \
  --set service.type=LoadBalancer
```

### Pull Chart Locally First
```bash
# Pull chart to local directory
helm pull oci://ghcr.io/sudoaaron/helm-charts/demo-app --version 0.2.2

# Install from local file
helm install my-demo demo-app-0.2.2.tgz
```

### Browse Available Charts
```bash
# Show chart information
helm show chart oci://ghcr.io/sudoaaron/helm-charts/demo-app --version 0.2.2

# Show default values
helm show values oci://ghcr.io/sudoaaron/helm-charts/demo-app --version 0.2.2

# Show all information
helm show all oci://ghcr.io/sudoaaron/helm-charts/demo-app --version 0.2.2
```

## 📁 Repository Structure

```
├── .github/workflows/          # GitHub Actions workflows
│   ├── lint-test.yml          # PR validation (lint, test, verify)
│   └── version-check.yml      # Combined: Auto-tag, release, and publish
├── charts/                    # Helm charts directory
│   └── demo-app/             # Example application chart
│       ├── Chart.yaml        # Chart metadata
│       ├── values.yaml       # Default values
│       ├── templates/        # Kubernetes templates
│       └── tests/           # Unit tests for the chart
└── ct.yaml                   # Chart testing configuration
```

## 🔄 Release Workflows

### 📝 **Primary Flow: Auto-Release on Main Merge**
1. **Create PR** with chart changes
2. **CI runs**: Lint, test, verify charts
3. **Merge to main**: Version detection triggers
4. **Auto-tag**: Creates `{chart-name}-v{version}` tags
5. **Auto-release**: Publishes to OCI registry + GitHub Releases

### 🚨 **Emergency Flow: Manual Tag Release**
```bash
# Create manual tag for hotfix/re-release
git tag demo-app-v0.2.1
git push origin demo-app-v0.2.1
# Triggers immediate release workflow
```

### 🎛️ **Manual Flow: Workflow Dispatch**
- Go to **Actions** → **Version Check, Tag, and Release**
- Click **Run workflow**
- Optionally specify chart name for single-chart release

## 🔄 Workflow Details

### 1. Pull Request Validation (`lint-test.yml`)
**Triggers**: PRs to main with chart changes
- **Chart linting** with `helm lint` and `ct lint`
- **Unit testing** with chart-testing
- **Integration testing** on kind cluster
- **Verification** of template rendering

### 2. Automated Release (`version-check.yml`)
**Triggers**: 
- Push to main (auto-tag + release)
- Manual tag push (release existing tag)  
- Workflow dispatch (manual control)

**Actions**:
- **Version detection**: Scans chart versions vs existing tags
- **Tag creation**: Auto-creates missing version tags
- **OCI publishing**: Pushes charts to `ghcr.io`
- **GitHub Releases**: Creates releases with chart packages

## 🛠 Setup Instructions

### 1. Repository Setup

1. **Use this template** to create a new repository
2. **No additional setup required** - GitHub Container Registry is enabled by default

### 2. Adding New Charts

1. **Create your chart** (or modify the demo-app):
   ```bash
   # Create a new chart
   cd charts/
   helm create my-app
   # Edit Chart.yaml, values.yaml, templates, etc.
   ```

2. **Bump version** in `Chart.yaml`:
   ```yaml
   version: 1.0.0  # This will trigger auto-release
   ```

3. **Submit PR**:
   ```bash
   git checkout -b feature/add-my-app
   git add .
   git commit -m "feat: add my-app chart v1.0.0"
   git push origin feature/add-my-app
   # Create PR in GitHub UI
   ```

4. **Merge to main** → Automatic release!
   helm create charts/my-app
   
   # Or modify the existing demo-app
   cd charts/demo-app
   # Edit Chart.yaml, values.yaml, templates, etc.
   ```

2. **Update chart version** in `Chart.yaml`:
   ```yaml
   version: 0.1.0  # This will trigger auto-tagging
   ```

3. **Commit and push** to main:
   ```bash
   git add .
   git commit -m "feat: add my-app chart v0.1.0"
   git push origin main
   ```

4. The automation will:
   - Create tag `my-app-v0.1.0`
   - Package and push the chart to OCI registry
   - Create a GitHub Release with installation instructions

### 3. Using the Published Charts

Once the workflow completes, your charts will be available at:
```
oci://ghcr.io/{username}/helm-charts/{chart-name}
```

To install:
```bash
# Install latest version
helm install my-release oci://ghcr.io/{username}/helm-charts/demo-app

# Install specific version
helm install my-release oci://ghcr.io/{username}/helm-charts/demo-app --version 0.1.0
```

## 📋 Development Guidelines

### Chart Versioning
- Follow [Semantic Versioning](https://semver.org/)
- Update the `version` field in `Chart.yaml` for releases
- Use `appVersion` to track the application version

### Testing
- Add unit tests in `charts/{chart-name}/tests/`
- Use [helm-unittest](https://github.com/helm-unittest/helm-unittest) syntax
- Integration tests run automatically in CI

### Chart Structure
Follow Helm best practices:
- Use template helpers in `_helpers.tpl`
- Provide sensible defaults in `values.yaml`
- Include proper labels and annotations
- Add resource limits and health checks

## 🧪 Local Development

### Prerequisites
- [Helm 3.8+](https://helm.sh/docs/intro/install/) (for OCI support)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [kind](https://kind.sigs.k8s.io/) (for local testing)
- [chart-testing](https://github.com/helm/chart-testing)

### Testing Charts Locally

```bash
# Install helm-unittest plugin
helm plugin install https://github.com/helm-unittest/helm-unittest

# Run unit tests
helm unittest charts/demo-app/

# Lint charts
helm lint charts/demo-app/

# Package chart
helm package charts/demo-app/

# Test installation (requires cluster)
helm install test-release charts/demo-app/
```

### Using Chart Testing

```bash
# Install chart-testing
pip install chart-testing

# Create kind cluster
kind create cluster

# Run chart testing
ct lint --config ct.yaml
ct install --config ct.yaml
```

## 🔍 Registry Information

- **Registry**: `ghcr.io/sudoaaron/helm-charts`
- **Visibility**: Public (accessible to everyone)
- **Authentication**: Not required for public charts
- **Versions**: Available through GitHub Container Registry UI

## 🆘 Troubleshooting

### Common Issues

**Charts not being released:**
- Check that the version in `Chart.yaml` has changed
- Verify workflow logs for errors
- Ensure Helm 3.8+ for OCI support

**Installation errors:**
- Verify chart syntax with `helm lint`
- Check template rendering with `helm template`
- Ensure Kubernetes cluster is accessible

### Getting Help

- Check the [Issues](../../issues) for common problems
- Review [Helm OCI documentation](https://helm.sh/docs/topics/registries/)
- See [GitHub Container Registry docs](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry)

## 📄 License

This template is provided under the MIT License. See LICENSE file for details.