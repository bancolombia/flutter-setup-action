# Flutter Setup Action

A GitHub Action to install Flutter SDK with intelligent caching support for faster CI/CD workflows.

## Features

- **Multi-channel support**: Stable, beta, and dev channels
- **Version flexibility**: Install specific versions or latest from channel
- **Intelligent caching**: Caches Flutter SDK, pub packages, and Gradle dependencies
- **Java integration**: Optional Temurin Java setup for Android development
- **Cross-platform**: Works on Linux, macOS, and Windows runners
- **Optimized performance**: Precaches Flutter artifacts and manages dependencies

## Usage

### Basic Setup

```yaml
- name: Setup Flutter
  uses: bancolombia/flutter-setup-action@v1
```

### Advanced Configuration

```yaml
- name: Setup Flutter
  uses: bancolombia/flutter-setup-action@v1
  with:
    channel: 'stable'
    flutter-version: '3.32.0'
    java-version: '17'
    cache: 'true'
    cache-key-suffix: 'v1'
```

### Complete Workflow Example

```yaml
name: Flutter CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup Flutter
      uses: bancolombia/flutter-setup-action@v1
      with:
        channel: 'stable'
        flutter-version: 'latest'
        java-version: '17'
        cache: 'true'
    
    - name: Get dependencies
      run: flutter pub get
    
    - name: Run tests
      run: flutter test
    
    - name: Build APK
      run: flutter build apk
```

## Inputs

| Input | Description | Required | Default | Examples |
|-------|-------------|----------|---------|----------|
| `channel` | Flutter channel to install | No | `stable` | `stable`, `beta`, `dev` |
| `flutter-version` | Exact Flutter version or "latest" | No | `latest` | `3.32.0`, `latest` |
| `java-version` | Temurin Java version (empty to skip) | No | `17` | `11`, `17`, `21` |
| `cache` | Enable caching for faster builds | No | `true` | `true`, `false` |
| `cache-key-suffix` | Suffix for manual cache invalidation | No | `""` | `v1`, `2024-01` |


## Platform Support

| Platform | Status | Notes |
|----------|--------|-------|
| Ubuntu (Linux) | Full support | Includes desktop development dependencies |
| macOS | Full support | iOS and macOS development ready |
| Windows | Full support | Windows app development ready |

## Dependencies

### Linux
The action automatically installs required Linux dependencies:
- Build essentials: `clang`, `cmake`, `ninja-build`
- Desktop development: `libgtk-3-dev`, `libglu1-mesa`
- Standard utilities: `curl`, `git`, `unzip`, `xz-utils`, `zip`

### Java Setup
When `java-version` is specified, the action uses `actions/setup-java@v4` with Temurin distribution.


## Troubleshooting

### Cache Issues
If you encounter cache-related problems, use `cache-key-suffix` to force cache invalidation:

```yaml
- uses: bancolombia/flutter-setup-action@v1
  with:
    cache-key-suffix: 'rebuild-2024-01'
```

### Version Conflicts
Ensure your `flutter-version` is available in the specified `channel`. Check [Flutter releases](https://docs.flutter.dev/development/tools/sdk/releases) for compatibility.

### Java Version Compatibility
- Flutter 3.3+: Requires Java 11 or higher
- Android Gradle Plugin 8.0+: Requires Java 17 or higher

## Contributing

We welcome contributions! Please see our [contributing guidelines](CONTRIBUTING.md) for details.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

For support and questions:
- Open an issue in this repository
- Check existing issues for similar problems
- Review Flutter's official documentation

## Changelog

See [RELEASES](https://github.com/bancolombia/flutter-setup-action/releases) for version history and changes.
