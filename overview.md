# Pin Specific Maven Version

Automatically pins a specific Maven version in your Azure Pipelines to ensure consistent builds across all environments.

## Overview

This Azure Pipelines extension provides a pipeline decorator that automatically installs and configures Maven version 3.9.11 before your pipeline jobs execute. It ensures that all builds use the same Maven version regardless of the agent's default configuration, eliminating "works on my machine" issues related to Maven version discrepancies.

## Why Use This Extension?

### Consistent Build Environment
- **Version Lock**: Guarantees all builds use Maven 3.9.11, preventing version-related build failures
- **Reproducible Builds**: Ensures builds are reproducible across different build agents and environments
- **No Manual Configuration**: Automatically applies to all pipelines without requiring changes to your YAML files

### Time-Saving Benefits
- **Zero Configuration**: Works immediately after installation - no pipeline changes needed
- **Automatic Setup**: Installs Maven before every job execution automatically
- **Agent Agnostic**: Works on any Linux-based build agent

### Build Reliability
- **Eliminates Version Drift**: Prevents issues caused by different Maven versions on different agents
- **Predictable Behavior**: Maven commands behave consistently across all builds
- **Dependency Resolution Stability**: Ensures consistent dependency resolution

## How It Works

This extension uses an Azure Pipelines [pipeline decorator](https://docs.microsoft.com/en-us/azure/devops/extend/develop/add-pipeline-decorator) that runs as a pre-job task. Before your pipeline job starts:

1. **Downloads Maven**: Fetches Maven 3.9.11 from Apache's official archive
2. **Installs to Agent**: Extracts Maven to the agent's tools directory
3. **Configures Environment**: Sets `M2_HOME`, `MAVEN_HOME`, and updates `PATH`
4. **Proceeds with Build**: Your pipeline job runs with the pinned Maven version

The decorator runs automatically on all pipelines in your Azure DevOps organization once installed.

## Features

✅ **Automatic Installation**: Maven 3.9.11 is installed before every pipeline job  
✅ **Environment Configuration**: Properly sets all required Maven environment variables  
✅ **Path Management**: Adds Maven to PATH for immediate command availability  
✅ **Agent Tools Integration**: Uses agent tools directory for efficient caching  
✅ **Non-Intrusive**: No changes to your existing pipeline YAML required  
✅ **Organization-Wide**: Applies to all pipelines in your organization

## Installation

1. Navigate to the [Visual Studio Marketplace](https://marketplace.visualstudio.com/)
2. Search for "Pin Specific Maven Version"
3. Click **Get it free**
4. Select your Azure DevOps organization
5. Click **Install**

That's it! All your pipelines will now use Maven 3.9.11.

## Usage

Once installed, the extension works automatically. No additional configuration is required.

### Verification

To verify Maven is correctly installed in your pipeline, add this step:

```yaml
- script: mvn -version
  displayName: 'Verify Maven Version'
```

Expected output:
```
Apache Maven 3.9.11
```

### Example Pipeline

Your existing pipeline doesn't need any changes:

```yaml
trigger:
  - main

pool:
  vmImage: 'ubuntu-latest'

steps:
  # Maven 3.9.11 is automatically installed here by the decorator
  
  - task: Maven@3
    inputs:
      mavenPomFile: 'pom.xml'
      goals: 'clean install'
```

## Technical Details

### Maven Version
- **Version**: 3.9.11
- **Source**: Apache Maven Official Archive
- **Download URL**: `https://archive.apache.org/dist/maven/maven-3/3.9.11/binaries/apache-maven-3.9.11-bin.tar.gz`

### Environment Variables Set
- `M2_HOME`: Points to the Maven installation directory
- `MAVEN_HOME`: Points to the Maven installation directory
- `PATH`: Prepended with Maven's bin directory

### Agent Compatibility
- **Supported**: Linux-based agents (ubuntu-latest, ubuntu-20.04, ubuntu-22.04, etc.)
- **Shell**: Bash script execution

## FAQ

### Q: Will this override my existing Maven configuration?
**A:** Yes, this extension prepends the Maven 3.9.11 path to your PATH variable, ensuring this version takes precedence.

### Q: Can I use a different Maven version?
**A:** This extension is specifically designed for Maven 3.9.11. If you need a different version, you can fork and modify the extension, or use the Maven task's built-in version specification.

### Q: Does this work on Windows agents?
**A:** Currently, this extension is designed for Linux-based agents. Windows agent support would require modifications to the installation script.

### Q: Will this slow down my builds?
**A:** The first run on an agent will download Maven (~9MB). Subsequent runs on the same agent may benefit from caching, making the overhead minimal.

### Q: How do I disable this for a specific pipeline?
**A:** Pipeline decorators apply organization-wide. To disable, you would need to uninstall the extension or modify the decorator to include conditions.

## Support

For issues, questions, or contributions:
- **Issues**: Report issues in the extension's repository
- **Documentation**: Refer to [Azure DevOps Pipeline Decorators](https://docs.microsoft.com/en-us/azure/devops/extend/develop/add-pipeline-decorator)

## License

ISC License

## Version History

### 1.0.0
- Initial release
- Maven 3.9.11 support
- Linux agent support
- Pre-job task decorator implementation

---

**Note**: This extension requires an active Azure DevOps organization and appropriate permissions to install extensions.
