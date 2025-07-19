# Helm Charts Repository

A template repository for building and managing Helm charts with automated CI/CD using GitHub Actions and GitHub Pages.

## 🚀 Features

- **Automated Testing**: Pull requests trigger linting, unit tests, and integration tests
- **Version Management**: Automatic tagging when chart versions change
- **Release Automation**: Tagged releases automatically package and publish charts
- **GitHub Pages**: Helm repository hosted on GitHub Pages for easy consumption
- **Demo Application**: Includes a sample nginx-based application chart

## 📁 Repository Structure

```
├── .github/workflows/          # GitHub Actions workflows
│   ├── lint-test.yml          # PR validation (lint, test, verify)
│   ├── version-check.yml      # Auto-tagging on version changes
│   └── release.yml            # Release and publish charts
├── charts/                    # Helm charts directory
│   └── demo-app/             # Example application chart
│       ├── Chart.yaml        # Chart metadata
│       ├── values.yaml       # Default values
│       ├── templates/        # Kubernetes templates
│       └── tests/           # Unit tests for the chart
├── ct.yaml                   # Chart testing configuration
└── index.yaml              # Helm repository index
```

## 🔄 Workflow Overview

### 1. Pull Request to Main
When you create a PR to the main branch with chart changes:
- **Linting**: Charts are linted for best practices
- **Unit Testing**: Helm unit tests are executed
- **Integration Testing**: Charts are deployed to a kind cluster
- **Verification**: All tests must pass before merge

### 2. Merge to Main
When changes are merged to main:
- **Version Detection**: Checks if any chart versions have changed
- **Auto-Tagging**: Creates git tags in format `{chart-name}-v{version}`
- **Trigger Release**: Automatically triggers the release workflow

### 3. Tag Push (vX.Y.Z)
When a tag is pushed (or created automatically):
- **Package Chart**: Creates a `.tgz` package of the chart
- **Update Repository**: Updates the Helm repository index
- **Deploy to GitHub Pages**: Publishes the repository for consumption

## 🛠 Setup Instructions

### 1. Repository Setup

1. **Use this template** to create a new repository
2. **Enable GitHub Pages** in your repository settings:
   - Go to Settings → Pages
   - Source: Deploy from a branch
   - Branch: `gh-pages` / root

3. **Configure repository permissions**:
   - Go to Settings → Actions → General
   - Workflow permissions: Read and write permissions
   - Allow GitHub Actions to create and approve pull requests: ✅

### 2. Initial Chart Release

1. **Create your first chart** (or modify the demo-app):
   ```bash
   # Create a new chart
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
   - Package and release the chart
   - Update the Helm repository

### 3. Using the Published Charts

Once the workflow completes, your charts will be available at:
```
https://{username}.github.io/{repository-name}/
```

To use the charts:
```bash
# Add the repository
helm repo add my-charts https://{username}.github.io/{repository-name}/

# Update repository
helm repo update

# Install a chart
helm install my-release my-charts/demo-app
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
- [Helm 3.x](https://helm.sh/docs/intro/install/)
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

## 🔧 Customization

### Adding New Charts

1. Create a new chart directory:
   ```bash
   helm create charts/new-chart
   ```

2. Customize the chart according to your needs

3. Add unit tests in `charts/new-chart/tests/`

4. Update the version in `Chart.yaml` and commit

### Modifying Workflows

The GitHub Actions workflows are in `.github/workflows/`:
- `lint-test.yml`: Customize testing and validation
- `version-check.yml`: Modify auto-tagging logic
- `release.yml`: Adjust release and publishing process

### Chart Testing Configuration

Modify `ct.yaml` to customize chart testing behavior:
- Add chart repositories
- Configure test parameters
- Set validation rules

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

The automated workflows will validate your changes before merge.

## 📄 License

This template is provided under the MIT License. See LICENSE file for details.

## 🆘 Troubleshooting

### Common Issues

**Charts not being released:**
- Check that the version in `Chart.yaml` has changed
- Verify GitHub Pages is enabled
- Check workflow logs for errors

**Tests failing:**
- Ensure unit tests are valid
- Check chart syntax with `helm lint`
- Verify template rendering with `helm template`

**Permission errors:**
- Ensure repository has write permissions for Actions
- Check that GitHub Pages is properly configured

### Getting Help

- Check the [Issues](../../issues) for common problems
- Review [Helm documentation](https://helm.sh/docs/)
- See [GitHub Actions documentation](https://docs.github.com/en/actions)