# Contributing to Helm Charts Repository

Thank you for your interest in contributing to this Helm charts repository! This document provides guidelines and information for contributors.

## 📋 Getting Started

### Prerequisites

Before contributing, ensure you have:
- [Helm 3.x](https://helm.sh/docs/intro/install/) installed
- [kubectl](https://kubernetes.io/docs/tasks/tools/) for testing
- [Git](https://git-scm.com/) for version control
- A GitHub account

### Setting Up Development Environment

1. **Fork and clone the repository:**
   ```bash
   git clone https://github.com/your-username/helm-charts.git
   cd helm-charts
   ```

2. **Install development tools:**
   ```bash
   # Install helm-unittest plugin
   helm plugin install https://github.com/helm-unittest/helm-unittest

   # Install chart-testing (optional, for advanced testing)
   pip install chart-testing
   ```

## 🛠 Development Workflow

### Creating a New Chart

1. **Create the chart structure:**
   ```bash
   helm create charts/my-new-chart
   ```

2. **Customize the chart:**
   - Edit `Chart.yaml` with proper metadata
   - Configure `values.yaml` with sensible defaults
   - Update templates in `templates/` directory
   - Add helper functions in `templates/_helpers.tpl`

3. **Add tests:**
   - Create unit tests in `tests/` directory
   - Use helm-unittest syntax
   - Test various scenarios and edge cases

### Modifying Existing Charts

1. **Update the chart version:**
   ```yaml
   # In Chart.yaml
   version: x.y.z  # Follow semantic versioning
   ```

2. **Make your changes:**
   - Update templates, values, or documentation
   - Ensure backward compatibility when possible
   - Update tests to cover new functionality

3. **Test your changes:**
   ```bash
   # Run unit tests
   helm unittest charts/my-chart/

   # Lint the chart
   helm lint charts/my-chart/

   # Test template rendering
   helm template my-release charts/my-chart/
   ```

## ✅ Quality Standards

### Chart Requirements

- **Metadata**: Proper `Chart.yaml` with description, maintainers, and keywords
- **Documentation**: Clear `README.md` for each chart
- **Values**: Well-documented `values.yaml` with comments
- **Templates**: Follow Helm best practices and conventions
- **Tests**: Unit tests covering main functionality
- **Security**: No hardcoded secrets, proper RBAC

### Code Style

- **YAML**: Use 2-space indentation
- **Templates**: Use meaningful variable names
- **Comments**: Add explanatory comments for complex logic
- **Helpers**: Use template helpers for reusable code

### Versioning

Follow [Semantic Versioning](https://semver.org/):
- **MAJOR**: Incompatible API changes
- **MINOR**: New functionality (backward compatible)
- **PATCH**: Bug fixes (backward compatible)

## 🧪 Testing

### Required Tests

1. **Unit Tests**: Test template rendering with various values
2. **Lint Tests**: Ensure charts follow best practices
3. **Installation Tests**: Verify charts install successfully

### Running Tests

```bash
# Unit tests
helm unittest charts/chart-name/

# Lint tests
helm lint charts/chart-name/

# Template tests
helm template test-release charts/chart-name/ --values charts/chart-name/values.yaml

# Full chart testing (requires cluster)
ct lint --config ct.yaml
ct install --config ct.yaml
```

### Test Examples

```yaml
# Example unit test (charts/chart-name/tests/deployment_test.yaml)
suite: test deployment
templates:
  - deployment.yaml
tests:
  - it: should render deployment
    asserts:
      - isKind:
          of: Deployment
      - equal:
          path: metadata.name
          value: RELEASE-NAME-chart-name
```

## 📝 Pull Request Process

### Before Submitting

1. **Test locally:**
   ```bash
   # Run all tests
   helm unittest charts/*/
   helm lint charts/*/
   ```

2. **Update documentation:**
   - Update chart README if needed
   - Add changelog entries for significant changes
   - Update version in Chart.yaml

3. **Commit guidelines:**
   - Use conventional commit messages
   - Keep commits focused and atomic
   - Sign your commits (optional but recommended)

### PR Requirements

- [ ] Chart version updated (if applicable)
- [ ] Tests pass locally
- [ ] Documentation updated
- [ ] No hardcoded values or secrets
- [ ] Backward compatibility maintained (or breaking changes documented)

### PR Template

```markdown
## Description
Brief description of changes made.

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Charts Modified
- [ ] chart-name-1
- [ ] chart-name-2

## Testing
- [ ] Unit tests pass
- [ ] Lint tests pass
- [ ] Tested installation on cluster

## Checklist
- [ ] Version updated in Chart.yaml
- [ ] Tests added/updated
- [ ] Documentation updated
- [ ] Breaking changes documented
```

## 🚀 Release Process

The release process is automated:

1. **Merge PR to main**: After review and approval
2. **Auto-tagging**: System detects version changes and creates tags
3. **Automated release**: Charts are packaged and published
4. **Repository update**: Helm repository index is updated

## 🤝 Community Guidelines

### Communication

- Be respectful and constructive in discussions
- Use clear, descriptive titles for issues and PRs
- Provide detailed information when reporting bugs
- Help others in discussions and reviews

### Issue Reporting

When reporting issues:
- Use the issue templates
- Provide clear reproduction steps
- Include relevant environment information
- Search existing issues first

### Feature Requests

- Describe the use case clearly
- Explain why existing solutions don't work
- Be open to alternative approaches
- Consider contributing the feature yourself

## 📚 Resources

### Helm Resources
- [Helm Documentation](https://helm.sh/docs/)
- [Chart Best Practices](https://helm.sh/docs/chart_best_practices/)
- [Template Functions](https://helm.sh/docs/chart_template_guide/)

### Testing Resources
- [Helm Unittest](https://github.com/helm-unittest/helm-unittest)
- [Chart Testing](https://github.com/helm/chart-testing)

### Kubernetes Resources
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [API Reference](https://kubernetes.io/docs/reference/kubernetes-api/)

## ❓ Getting Help

- **GitHub Issues**: For bugs and feature requests
- **GitHub Discussions**: For questions and general discussion
- **Documentation**: Check the repository README and chart documentation

Thank you for contributing to make this Helm charts repository better! 🎉
