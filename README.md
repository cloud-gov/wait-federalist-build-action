# wait-federalist-build-action

Wait on a [Federalist](https://federalist.18f.gov/) build to succeed and return the build preview URL.

## Defaults

The code will query for the Federalist build status every 30 seconds and retry up to 20 times, thus waiting for
a successful Federalist build for a total of 10 minutes.

## Inputs

- `debug`: Set to any value other than `false` if you want to emit debugging output
- `commit-sha`: Commit SHA to query for a Federalist build.

## Outputs

- `preview-url`: The full URL to the Federalist preview build for the commit (e.g. `https://1234-abcd-5678-efgh.app.domain/preview/your-org/your-repo/your-branch/`)

## Example

Add a step using this action to your workflow:

```yaml
      - name: Wait on federalist build
        id: wait-federalist-build
        uses: cloud-gov/wait-federalist-build-action@main
        with:
          debug: true
          commit-sha: ${{ github.event.pull_request.head.sha }}
```

**Note:** In the context of a pull request, `${{ github.event.pull_request.head.sha }}` is what you want to get the
latest commit on your branch, not `${{ github.sha }}`.

Then, if the step succeeds, subsequent steps will have access to the build preview URL in `${{ steps.wait-federalist-build.outputs.preview-url }}`.

## Testing locally

If you want to run the script used by the action to test it locally:

```shell
    GITHUB_API_URL=https://api.github.com \
        GITHUB_REPOSITORY=your-org/your-repo \
        COMMIT_SHA=some-sha \
        DEBUG=true \
        bash wait-for-federalist-build.sh
```
