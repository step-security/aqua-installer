[![StepSecurity Maintained Action](https://raw.githubusercontent.com/step-security/maintained-actions-assets/main/assets/maintained-action-banner.png)](https://docs.stepsecurity.io/actions/stepsecurity-maintained-actions)

# aqua-installer

[![GitHub last commit](https://img.shields.io/github/last-commit/step-security/aqua-installer.svg)](https://github.com/step-security/aqua-installer)
[![License](http://img.shields.io/badge/license-mit-blue.svg?style=flat-square)](https://raw.githubusercontent.com/step-security/aqua-installer/main/LICENSE)

Install [aqua](https://aquaproj.github.io/) quickly.

## Shell Script

Install aqua with a single command:

```bash
curl -sSfL https://raw.githubusercontent.com/step-security/aqua-installer/v4.0.4/aqua-installer | bash
```

### Secure Installation with Checksum Verification

For safer installation, verify the checksum before execution:

```bash
curl -sSfL -O https://raw.githubusercontent.com/step-security/aqua-installer/v4.0.4/aqua-installer
echo "98b883756cdd0a6807a8c7623404bfc3bc169275ad9064dc23a6e24ad398f43d  aqua-installer" | sha256sum -c -
chmod +x aqua-installer
./aqua-installer
```

### Installation Paths

- **Linux, macOS:** `${AQUA_ROOT_DIR:-${XDG_DATA_HOME:-$HOME/.local/share}/aquaproj-aqua}/bin/aqua`
- **Windows:** `${AQUA_ROOT_DIR:-$HOME/AppData/Local/aquaproj-aqua}/bin/aqua`

⚠️ From version 2 onward, custom installation paths are not supported.

### Parameters

- `-v [version]`: Specify aqua version (defaults to latest if omitted)

Example with version specification:

```bash
curl -sSfL https://raw.githubusercontent.com/step-security/aqua-installer/v4.0.4/aqua-installer | bash -s -- -v v2.43.1
```

## GitHub Actions

### Basic Usage

```yaml
- uses: step-security/aqua-installer@v4
  with:
    aqua_version: v2.43.1
```

### Extended Configuration

```yaml
- uses: step-security/aqua-installer@v4
  with:
    aqua_version: v2.43.1
    working_directory: foo
    aqua_opts: ""
  env:
    AQUA_CONFIG: aqua-config.yaml
    AQUA_LOG_LEVEL: debug
```

### Required Inputs

| Name | Description |
|------|-------------|
| `aqua_version` | Installed aqua version |

### Optional Inputs

| Name | Default | Description |
|------|---------|-------------|
| `skip_install_aqua` | `"false"` | Skip installation if aqua exists (v3.1.0+) |
| `enable_aqua_install` | `"true"` | Skip `aqua i` command if `"false"` |
| `aqua_opts` | `-l` | Options for `aqua i` command |
| `working_directory` | `""` | Working directory for execution |
| `policy_allow` | `""` | Enable policy enforcement (v2.3.0+) |

### Outputs

No outputs are provided.

## Caching Strategy

While aqua-installer lacks built-in caching, you can cache packages using `actions/cache`:

```yaml
- uses: actions/cache@v5
  with:
    path: ~/.local/share/aquaproj-aqua
    key: v2-aqua-installer-${{runner.os}}-${{runner.arch}}-${{hashFiles('aqua.yaml')}}
    restore-keys: |
      v2-aqua-installer-${{runner.os}}-${{runner.arch}}-

- uses: step-security/aqua-installer@v4
  with:
    aqua_version: v2.43.1
```

**For split configurations**, include additional file hashes in cache keys.

**Default behavior:** Runs with `-l` option, caching only packages used in workflows. To cache all packages, unset this option:

```yaml
aqua_opts: ""
```

## License

[MIT](LICENSE)
