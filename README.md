# Arbeidstilsynet/action-dotnet-publish

A GitHub Action that packs and publishes a .NET package to a NuGet feed. Supports trusted publishing (OIDC) for nuget.org and authenticated publishing for other feeds.

## Requirements

- .NET project in the specified working directory
- **Trusted publishing (default):** A [trusted publishing policy](https://learn.microsoft.com/nuget/nuget-org/trusted-publishing) configured on nuget.org for your repository, and `id-token: write` permission on the job
- **Authenticated publishing:** A valid NuGet API key (provided as a secret)

## Inputs

| Name                     | Description                                                                | Required | Default                               |
|--------------------------|----------------------------------------------------------------------------|----------|---------------------------------------|
| `working-directory`      | The directory to run dotnet commands in                                    | Yes      |                                       |
| `use-trusted-publishing` | Use OIDC-based trusted publishing for nuget.org                            | No       | `true`                                |
| `nuget-username`         | Your nuget.org username (required when `use-trusted-publishing` is `true`) | No       |                                       |
| `nuget-auth-token`       | The NuGet auth token (required when `use-trusted-publishing` is `false`)   | No       |                                       |
| `source-url`             | The source URL for the NuGet package feed                                  | No       | `https://api.nuget.org/v3/index.json` |
| `dotnet-version`         | The version of dotnet to use                                               | No       | `10.0.x`                              |
| `config-file`            | Custom nuget.config location                                               | No       | `nuget.config` (at root)              |

## Usage

### Trusted publishing (default)

Publishes to nuget.org using OIDC — no API key needed. Requires [trusted publisher configuration](https://learn.microsoft.com/nb-no/nuget/nuget-org/trusted-publishing) on nuget.org.

```yaml
jobs:
  publish:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - uses: actions/checkout@v6

      - uses: Arbeidstilsynet/action-dotnet-publish@v3
        with:
          working-directory: ./src/YourProject
          nuget-username: ${{ vars.NUGET_BOT_USERNAME }}
```

### Authenticated publishing

Publishes to any feed (e.g. Azure Artifacts) using an API key. You must provide `source-url` for non-nuget.org feeds.

```yaml
jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v6

      - uses: Arbeidstilsynet/action-dotnet-publish@v3
        with:
          working-directory: ./src/YourProject
          use-trusted-publishing: "false"
          nuget-auth-token: ${{ secrets.NUGET_AUTH_TOKEN }}
          source-url: https://pkgs.dev.azure.com/MyOrg/MyProject/_packaging/MyFeed/nuget/v3/index.json
```

## Versioning

This repository uses a simple versioning system based on the `VERSION` file.
When you update the `VERSION` file and push to `main`, a Git tag with that version is created or updated automatically by the workflow.
If you make breaking changes to the action, bump the version and update `CHANGELOG.md`.
