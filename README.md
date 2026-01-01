# aqua-installer

[![DeepWiki](https://img.shields.io/badge/DeepWiki-aquaproj%2Faqua--installer-blue.svg?logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACwAAAAyCAYAAAAnWDnqAAAAAXNSR0IArs4c6QAAA05JREFUaEPtmUtyEzEQhtWTQyQLHNak2AB7ZnyXZMEjXMGeK/AIi+QuHrMnbChYY7MIh8g01fJoopFb0uhhEqqcbWTp06/uv1saEDv4O3n3dV60RfP947Mm9/SQc0ICFQgzfc4CYZoTPAswgSJCCUJUnAAoRHOAUOcATwbmVLWdGoH//PB8mnKqScAhsD0kYP3j/Yt5LPQe2KvcXmGvRHcDnpxfL2zOYJ1mFwrryWTz0advv1Ut4CJgf5uhDuDj5eUcAUoahrdY/56ebRWeraTjMt/00Sh3UDtjgHtQNHwcRGOC98BJEAEymycmYcWwOprTgcB6VZ5JK5TAJ+fXGLBm3FDAmn6oPPjR4rKCAoJCal2eAiQp2x0vxTPB3ALO2CRkwmDy5WohzBDwSEFKRwPbknEggCPB/imwrycgxX2NzoMCHhPkDwqYMr9tRcP5qNrMZHkVnOjRMWwLCcr8ohBVb1OMjxLwGCvjTikrsBOiA6fNyCrm8V1rP93iVPpwaE+gO0SsWmPiXB+jikdf6SizrT5qKasx5j8ABbHpFTx+vFXp9EnYQmLx02h1QTTrl6eDqxLnGjporxl3NL3agEvXdT0WmEost648sQOYAeJS9Q7bfUVoMGnjo4AZdUMQku50McDcMWcBPvr0SzbTAFDfvJqwLzgxwATnCgnp4wDl6Aa+Ax283gghmj+vj7feE2KBBRMW3FzOpLOADl0Isb5587h/U4gGvkt5v60Z1VLG8BhYjbzRwyQZemwAd6cCR5/XFWLYZRIMpX39AR0tjaGGiGzLVyhse5C9RKC6ai42ppWPKiBagOvaYk8lO7DajerabOZP46Lby5wKjw1HCRx7p9sVMOWGzb/vA1hwiWc6jm3MvQDTogQkiqIhJV0nBQBTU+3okKCFDy9WwferkHjtxib7t3xIUQtHxnIwtx4mpg26/HfwVNVDb4oI9RHmx5WGelRVlrtiw43zboCLaxv46AZeB3IlTkwouebTr1y2NjSpHz68WNFjHvupy3q8TFn3Hos2IAk4Ju5dCo8B3wP7VPr/FGaKiG+T+v+TQqIrOqMTL1VdWV1DdmcbO8KXBz6esmYWYKPwDL5b5FA1a0hwapHiom0r/cKaoqr+27/XcrS5UwSMbQAAAABJRU5ErkJggg==)](https://deepwiki.com/step-security/aqua-installer) [![GitHub last commit](https://img.shields.io/github/last-commit/step-security/aqua-installer.svg)](https://github.com/step-security/aqua-installer)
[![License](http://img.shields.io/badge/license-mit-blue.svg?style=flat-square)](https://raw.githubusercontent.com/step-security/aqua-installer/main/LICENSE)

Install [aqua](https://aquaproj.github.io/) quickly.

## Shell Script

Install aqua with a single command:

```bash
curl -sSfL https://raw.githubusercontent.com/aquaproj/aqua-installer/v4.0.2/aqua-installer | bash
```

### Secure Installation with Checksum Verification

For safer installation, verify the checksum before execution:

```bash
curl -sSfL -O https://raw.githubusercontent.com/aquaproj/aqua-installer/v4.0.2/aqua-installer
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
curl -sSfL https://raw.githubusercontent.com/aquaproj/aqua-installer/v4.0.2/aqua-installer | bash -s -- -v v2.43.1
```

## GitHub Actions

### Basic Usage

```yaml
- uses: aquaproj/aqua-installer@11dd79b4e498d471a9385aa9fb7f62bb5f52a73c # v4.0.4
  with:
    aqua_version: v2.43.1
```

### Extended Configuration

```yaml
- uses: aquaproj/aqua-installer@11dd79b4e498d471a9385aa9fb7f62bb5f52a73c # v4.0.4
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
- uses: actions/cache@9255dc7a253b0ccc959486e2bca901246202afeb # v5.0.1
  with:
    path: ~/.local/share/aquaproj-aqua
    key: v2-aqua-installer-${{runner.os}}-${{runner.arch}}-${{hashFiles('aqua.yaml')}}
    restore-keys: |
      v2-aqua-installer-${{runner.os}}-${{runner.arch}}-

- uses: aquaproj/aqua-installer@11dd79b4e498d471a9385aa9fb7f62bb5f52a73c # v4.0.4
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
