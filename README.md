# Pin Specific Maven Version

Azure Pipelines extension to pin a specific Maven version across all pipelines.

## Description

This extension automatically installs Maven 3.9.11 before running any job in Azure Pipelines, ensuring that all builds use the same Maven version regardless of the agent.

## Prerequisites

- Node.js installed
- An Azure DevOps account
- Permissions to publish extensions to the Marketplace

## Tool Installation

Install TFX CLI globally:

```bash
npm install -g tfx-cli
```

## Building the Extension

To generate the `.vsix` extension file:

```bash
npx tfx-cli extension create
```

This will create a file with the format `{publisher}.{extensionId}-{version}.vsix` in the current directory.

## Publishing to Marketplace

1. Create a publisher account at [Visual Studio Marketplace](https://marketplace.visualstudio.com/manage)

2. Update the `publisher` field in `vss-extension.json` with your publisher ID

3. Publish the extension:
```bash
npx tfx-cli extension publish --manifest-globs vss-extension.json --share-with {organization}
```

Or publish manually by uploading the `.vsix` file to the Marketplace portal.

## Project Structure

```
.
├── my-decorator.yml       # Pipeline decorator that installs Maven
├── vss-extension.json     # Extension manifest
├── overview.md           # Marketplace documentation
├── package.json          # Project dependencies
├── .gitignore           # Git ignored files
└── README.md            # This file
```

## How It Works

The extension uses a pipeline decorator that runs before each job:

1. Downloads Maven 3.9.11 from the official Apache repository
2. Extracts Maven to the agent's tools directory
3. Configures environment variables `M2_HOME`, `MAVEN_HOME`
4. Adds Maven to the `PATH`

## Local Development

1. Clone the repository
2. Install dependencies: `npm install`
3. Make changes to `my-decorator.yml` or `vss-extension.json`
4. Generate the extension: `npx tfx-cli extension create`
5. Test by installing the `.vsix` file in your Azure DevOps organization

## Configuration

The extension requires no additional configuration. Once installed in your Azure DevOps organization, it will automatically apply to all pipelines.

## Maven Version

Currently pins version **Maven 3.9.11**. To change the version, modify the `MAVEN_VERSION` variable in `my-decorator.yml`.

## License

ISC
