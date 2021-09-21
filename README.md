# virus-scan-action

This  action performs virus scanning on every published release, and is meant to be run using the release published event:

```yaml
name: Virus scan
on:
  release:
    types: [published]
```

The steps involved are:

1. Install ClamAV from apt-get
1. Update the virus signature database
1. Get the release's assets from the GitHub API, including the source zipball and tarball
1. Run ClamAV against all the assets
1. (Optionally) post to a Slack channel if a virus is detected
1. Update the release notes by appending information about the scan

## Usage

All inputs are required except for `slack-token` and `slack-channel`. If either is omitted, no posting to Slack will occur.

```yaml
name: Virus scan
on:
  release:
    types: [published]
jobs:
  virus-scan:
    runs-on: ubuntu-latest
    steps:
      - name: Scan release for viruses
        uses: Particular/virus-scan-action@main
        with:
          owner: Particular
          repo: REPOSITORY_NAME
          tag: ${{ github.event.release.name }}
          github-access-token: ${{ secrets.GITHUB_ACCESS_TOKEN }}
          slack-token: ${{ secrets.SLACK_TOKEN }}
          slack-channel: buildserver
```

## License

The scripts and documentation in this project are released under the [MIT License](LICENSE.md).