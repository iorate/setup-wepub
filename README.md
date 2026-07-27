# setup-wepub

A GitHub Action that installs the [`wepub`](https://github.com/iorate/wepub) CLI for publishing browser extensions to the Chrome Web Store, Firefox Add-ons, and Edge Add-ons.

It downloads the prebuilt binary from the wepub releases, verifies its build provenance attestation to ensure that it was built by the wepub release workflow on GitHub Actions, and adds it to the `PATH` so subsequent steps can call `wepub` directly. The binary is cached in the runner tool cache, so repeated runs of the same version skip the download.

## Requirements

The [GitHub CLI](https://cli.github.com/) must be available on the runner (it is preinstalled on GitHub-hosted runners).

## Usage

```yaml
- uses: iorate/setup-wepub@v2

- run: wepub chrome ./my-extension.zip
  env:
    WEPUB_CHROME_PUBLISHER_ID: ${{ vars.WEPUB_CHROME_PUBLISHER_ID }}
    WEPUB_CHROME_ITEM_ID: ${{ vars.WEPUB_CHROME_ITEM_ID }}
    WEPUB_CHROME_CLIENT_ID: ${{ secrets.WEPUB_CHROME_CLIENT_ID }}
    WEPUB_CHROME_CLIENT_SECRET: ${{ secrets.WEPUB_CHROME_CLIENT_SECRET }}
    WEPUB_CHROME_REFRESH_TOKEN: ${{ secrets.WEPUB_CHROME_REFRESH_TOKEN }}
```

Credentials and IDs can be supplied as `WEPUB_*` environment variables, so they can come from Actions secrets and variables. See the [wepub documentation](https://github.com/iorate/wepub#usage) for the variable names.

Pin to a specific version instead of tracking the latest release:

```yaml
- uses: iorate/setup-wepub@v2
  with:
    version: 1.0.1
```

## Inputs

| Name      | Default               | Description                                                                 |
| --------- | --------------------- | --------------------------------------------------------------------------- |
| `version` | `latest`              | Version to install: `latest`, or a specific version such as `1.0.1` or `v1.0.1`. |
| `token`   | `${{ github.token }}` | GitHub token used to resolve `latest` and verify build provenance.          |

## Outputs

| Name      | Description                                                       |
| --------- | ----------------------------------------------------------------- |
| `version` | The version of wepub that was installed (without the leading `v`). |

## Supported versions

wepub 1.0.1 or later. Earlier releases have no build provenance attestations and fail verification; use `iorate/setup-wepub@v1` to install them.

## Supported runners

| OS      | Architectures      |
| ------- | ------------------ |
| Linux   | x86_64, aarch64    |
| macOS   | x86_64, aarch64    |
| Windows | x86_64             |

## License

[MIT](LICENSE)
