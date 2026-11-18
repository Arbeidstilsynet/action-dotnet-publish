# Changelog

## v3

Add support for NuGet trusted publishing (OIDC) as the default publishing method. The action now publishes to nuget.org without an API key by default. Authenticated publishing to other feeds (e.g. Azure Artifacts) is still supported by setting `use-trusted-publishing: "false"`.

### Breaking changes

- `nuget-auth-token` is no longer required by default — only needed when `use-trusted-publishing` is `"false"`
- Default `source-url` for trusted publishing is now `https://api.nuget.org/v3/index.json`
- Jobs using trusted publishing must set `permissions: id-token: write`

### Added

- Optional `global-json-file` input to install the .NET SDK version from a `global.json` (path relative to the repository root). When set, `dotnet-version` is ignored. Default behaviour is unchanged.

## v2

Bump default value for `dotnet-version` to `10.0.x`

## v1

Initial release
