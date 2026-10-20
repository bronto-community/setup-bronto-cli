<p align="center">
  <img src="https://raw.githubusercontent.com/bronto-community/setup-bronto-cli/main/assets/logomark.png" alt="Bronto logo" width="140">
</p>

# Setup bronto CLI

[![CI](https://github.com/bronto-community/setup-bronto-cli/actions/workflows/ci.yml/badge.svg)](https://github.com/bronto-community/setup-bronto-cli/actions/workflows/ci.yml)

GitHub Action that installs the [bronto CLI](https://github.com/bronto-community/bronto-cli)
and verifies the download, so your workflows can search logs, send events, and manage
monitors in [Bronto](https://bronto.io).

It works on Linux, macOS and Windows runners, on amd64 and arm64.

## What is Bronto?

[Bronto](https://bronto.io) is an observability platform for logs, metrics, and traces. It
queries petabytes of data in under a second, at full fidelity with no sampling, and includes
one year of hot retention. The bronto CLI is its open-source, community-maintained
command-line client: search and tail logs, explore traces, send events, and manage
resources such as datasets and monitors, from a terminal or a CI job.

New to Bronto? **[Start a free trial](https://bronto.io/signup)**, create an API key, and
store it as a repository secret (for example `BRONTO_API_KEY`).

## Usage

```yaml
steps:
  - uses: bronto-community/setup-bronto-cli@v1
    with:
      api-key: ${{ secrets.BRONTO_API_KEY }}
      region: eu

  - run: bronto search 'status >= 500' --since 1h
```

Pin a version of the bronto CLI for reproducible runs:

```yaml
  - uses: bronto-community/setup-bronto-cli@v1
    with:
      version: 0.8.1
```

## Inputs

| Input | Default | Description |
| --- | --- | --- |
| `version` | `latest` | Version of the bronto CLI, such as `0.8.1` or `v0.8.1`, or `latest`. |
| `verify-signature` | `true` | Also verify the cosign signature on the release's `checksums.txt`. The sha256 check always runs. |
| `api-key` | | Bronto API key. Masked in logs and exported as `BRONTO_API_KEY` for later steps. |
| `region` | | Bronto region, exported as `BRONTO_REGION` for later steps. |
| `github-token` | `${{ github.token }}` | Used only to look up the latest release without hitting API rate limits. |

Pick the narrowest key for the job: an ingestion-only key for jobs that only run
`bronto send`, a read-only key for jobs that search. See the bronto CLI's
[CI/CD guide](https://docs.bronto.io/cli/guides/ci-cd).

## Outputs

| Output | Description |
| --- | --- |
| `version` | The installed version, without a leading `v`. |
| `path` | Directory containing the `bronto` executable, which is added to `PATH`. |

## How the download is verified

1. The release archive's sha256 must match its entry in the release's `checksums.txt`.
2. With `verify-signature` (the default), `cosign verify-blob` checks the keyless
   signature on `checksums.txt`. The signing certificate must come from bronto-cli's
   `release.yml` workflow, run for exactly the requested tag. Releases v0.1.1 and
   earlier are unsigned; set `verify-signature: false` to install them.

## Reporting problems

Report problems with the action here. Report anything about the bronto CLI itself on
[bronto-cli's issue tracker](https://github.com/bronto-community/bronto-cli/issues).

## Support

This action and the bronto CLI are open-source community projects from Bronto: free to use
and open to contributions, but not covered by Bronto's product support.

## Versioning

Releases are tagged `vX.Y.Z`, and the major tag (`v1`) moves to the latest release in
that major version. Use `@v1` to get fixes automatically, or pin a full commit SHA.

## Trademarks

Bronto and the Bronto logo are trademarks of Bronto, used here with permission. See
[TRADEMARK.md](https://github.com/bronto-community/bronto-cli/blob/main/TRADEMARK.md) in
bronto-cli. The code in this repository is BSD-2-Clause licensed; the license does not
cover Bronto's marks.
