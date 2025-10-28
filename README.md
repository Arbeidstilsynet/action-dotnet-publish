# Arbeidstilsynet/action-dotnet-publish

A GitHub Action that packs and publishes a .NET package to a NuGet feed.

## Versioning

This repository uses a simple versioning system based on the `VERSION` file.
When you update the `VERSION` file and push to `main`, a Git tag with that version is created or updated automatically by the workflow.
If you have to make breaking changes to the action, bump the version.

## Requirements

- .NET project in the specified working directory
- A valid NuGet API key (provided as a secret)

## Inputs

| Name                | Description                               | Required | Default                                                                                           |
|---------------------|-------------------------------------------|----------|---------------------------------------------------------------------------------------------------|
| `working-directory` | The directory to run dotnet commands in   | Yes      |                                                                                                   |
| `nuget-auth-token`  | The NuGet auth token                      | Yes      |                                                                                                   |
| `source-url`        | The source URL for the NuGet package feed | No       | `https://pkgs.dev.azure.com/Atil-utvikling/Public/_packaging/AT.Public.NuGet/nuget/v3/index.json` |
| `dotnet-version`    | The version of dotnet to use              | No       | `8.0.x`                                                                                           |
| `config-file`       | Custom nuGet.config location              | No       | `NuGet.config` (at source-url)                                                                    |

## Usage

```yaml
jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: Arbeidstilsynet/action-dotnet-publish@v1
        with:
          working-directory: ./src/YourProject
          nuget-auth-token: ${{ secrets.NUGET_AUTH_TOKEN }}
```
