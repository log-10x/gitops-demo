# GitOps Demo Repository

This repository demonstrates the [GitOps-based configuration management](http://doc.log10x.com/architecture/gitops/) approach for Log10x deployments. Use this as a reference when creating your own configuration repository.

## Getting Started

You can either fork this repository or create your own clean repository. There's no need to keep it in sync with this upstream repository.

See the [GitOps Getting Started guide](http://doc.log10x.com/architecture/gitops/#getting-started) for detailed instructions.

## Repository Structure

Example structure for organizing your configuration files:

```
├── config/
│   ├── apps/
│   │   ├── edge/
│   │   │   ├── optimizer.yaml
│   │   │   ├── regulator.yaml
│   │   │   └── reporter.yaml
│   │   └── cloud/
│   │       ├── reporter.yaml
│   │       └── streamer.yaml
│   └── pipelines/
│       └── run/
│           └── modules/
├── scripts/
│   └── enrichment.js
├── lookups/
│   ├── geoip.mmdb
│   └── cost-policy.csv
└── symbols/
    └── myapp.10x.tar
```

- **`config/`**: YAML configuration files - see [Configuration Documentation](http://doc.log10x.com/config/)
- **`scripts/`**: JavaScript enrichment scripts - see [JavaScript API](http://doc.log10x.com/api/js/)
- **`lookups/`**: CSV/TSV lookup files and GeoIP databases - see [TenXLookup API](http://doc.log10x.com/api/js/#TenXLookup)
- **`symbols/`**: AOT-compiled symbol library files (`.10x.tar`) - see [Compiler application](http://doc.log10x.com/apps/compiler/)

## Example Configuration

Reference your repository in your Log10x deployment using the `@github` launch macro. See [GitHub Configuration](http://doc.log10x.com/config/github/) for complete details.

**YAML Configuration:**
```yaml
tenx: run

include:
  - source: github
    options:
      token: $=TenXEnv.get("GH_TOKEN")
      repo: my-org/my-config-repo
      branch: production
      paths:
        - config/*.yaml
        - scripts/*.js
        - lookups/*.csv
        - symbols/*.10x.tar
      syncInterval: 5min
```

**Docker CLI:**
```bash
docker run --rm \
  -e TENX_API_KEY=${TENX_API_KEY} \
  -e GH_TOKEN=${GH_TOKEN} \
  ghcr.io/log-10x/log10x-pipeline:latest \
  '@github={"token": "${GH_TOKEN}", "repo": "my-org/my-config-repo", "branch": "production"}' \
  @apps/cloud/reporter
```

**Kubernetes:**
See [Deployment Guide](http://doc.log10x.com/deploy/) for Kubernetes configuration examples.

## Best Practices

See the [GitOps Best Practices](http://doc.log10x.com/architecture/gitops/#best-practices) documentation for:
- Secret management
- Branch strategy
- Repository synchronization
- Version tagging
- Configuration validation

## Example Workflows

### Edge Policy → Edge Regulator

See the [Policy Module documentation](http://doc.log10x.com/run/regulate/policy/) for the complete workflow where policy lookup files are committed to this repository and pulled by edge regulators.

### Multi-Environment Deployment

Use separate branches for different environments. Each environment pulls from its respective branch automatically. See [GitOps Architecture](http://doc.log10x.com/architecture/gitops/) for details.

## Documentation

- [GitOps Architecture](http://doc.log10x.com/architecture/gitops/)
- [GitHub Configuration](http://doc.log10x.com/config/github/)
- [Application Configuration](http://doc.log10x.com/config/app/)
- [JavaScript API](http://doc.log10x.com/api/js/)
- [Deployment Guide](http://doc.log10x.com/deploy/)

## License

See [LICENSE](LICENSE) file for details.

