# go-webext-action

A GitHub Action that wraps [adguardteam/go-webext](https://github.com/adguardteam/go-webext).

## Quick start

```yaml
- uses: ninoseki/go-webext-action@v1
  with:
    command: update
    browser: chrome
    app-id: ${{ vars.CHROME_ITEM_ID }}
    file: ./dist/chrome.zip
  env:
    CHROME_CLIENT_ID: ${{ secrets.CHROME_CLIENT_ID }}
    CHROME_CLIENT_SECRET: ${{ secrets.CHROME_CLIENT_SECRET }}
    CHROME_REFRESH_TOKEN: ${{ secrets.CHROME_REFRESH_TOKEN }}
```

## Inputs

| Name | Required | Description |
|------|----------|-------------|
| `command` | yes | One of `status`, `insert`, `update`, `publish`, `sign`. |
| `browser` | yes | One of `chrome`, `firefox`, `edge`. |
| `version` | no | go-webext version to install. Default `latest`. |
| `go-version` | no | Go version for `actions/setup-go`. Default `stable`. Set to empty to skip and use Go already on PATH. |
| `cache` | no | Cache the installed go-webext binary across runs, keyed on resolved version + OS + arch. `true`/`false`. Default `false`. |
| `app-id` | no | Extension/item/product ID (`-a/--app`). |
| `file` | no | Path to the extension archive (`-f/--file`). |
| `source` | no | Path to the source archive — Firefox (`-s/--source`). |
| `channel` | no | Firefox update channel: `listed` or `unlisted` (`-c/--channel`). |
| `approval-notes` | no | Notes for Mozilla reviewers — Firefox `update`/`sign` (`-n/--approval-notes`). |
| `output` | no | Output path for the signed `.xpi` — Firefox `sign` (`-o/--output`). |
| `target` | no | Chrome v1 publish target: `trustedTesters` or `default` (`-t/--target`). |
| `expedited` | no | Chrome publish — request skip review (`-e/--expedited`). `true`/`false`. |
| `staged` | no | Chrome v2 publish — stage instead of publishing (`-s/--staged`). `true`/`false`. |
| `percentage` | no | Chrome publish — gradual rollout 0–100 (`-p/--percentage`). |
| `timeout` | no | Edge update — upload timeout in seconds (`-t/--timeout`). |
| `verbose` | no | Enable debug logging (`-v/--verbose`). `true`/`false`. |
| `args` | no | Extra raw arguments appended to the invocation, for flags this action doesn't yet expose. |
| `working-directory` | no | Directory to run `go-webext` in. Defaults to `${{ github.workspace }}`. |

### Chrome

```yaml
env:
  CHROME_CLIENT_ID: ${{ secrets.CHROME_CLIENT_ID }}
  CHROME_CLIENT_SECRET: ${{ secrets.CHROME_CLIENT_SECRET }}
  CHROME_REFRESH_TOKEN: ${{ secrets.CHROME_REFRESH_TOKEN }}
  # Optional, for v2 API:
  # CHROME_PUBLISHER_ID: ${{ secrets.CHROME_PUBLISHER_ID }}
  # CHROME_API_VERSION: v2
```

### Firefox

```yaml
env:
  FIREFOX_CLIENT_ID: ${{ secrets.FIREFOX_CLIENT_ID }}
  FIREFOX_CLIENT_SECRET: ${{ secrets.FIREFOX_CLIENT_SECRET }}
```

### Edge (v1.1, recommended)

```yaml
env:
  EDGE_CLIENT_ID: ${{ secrets.EDGE_CLIENT_ID }}
  EDGE_API_KEY: ${{ secrets.EDGE_API_KEY }}
  EDGE_API_VERSION: v1.1
```

## Examples

### Upload a new Chrome version

```yaml
- uses: ninoseki/go-webext-action@v1
  with:
    command: update
    browser: chrome
    app-id: ${{ vars.CHROME_ITEM_ID }}
    file: ./dist/chrome.zip
  env:
    CHROME_CLIENT_ID: ${{ secrets.CHROME_CLIENT_ID }}
    CHROME_CLIENT_SECRET: ${{ secrets.CHROME_CLIENT_SECRET }}
    CHROME_REFRESH_TOKEN: ${{ secrets.CHROME_REFRESH_TOKEN }}
```

### Publish to Chrome with gradual rollout

```yaml
- uses: ninoseki/go-webext-action@v1
  with:
    command: publish
    browser: chrome
    app-id: ${{ vars.CHROME_ITEM_ID }}
    percentage: '50'
    expedited: 'true'
  env:
    CHROME_CLIENT_ID: ${{ secrets.CHROME_CLIENT_ID }}
    CHROME_CLIENT_SECRET: ${{ secrets.CHROME_CLIENT_SECRET }}
    CHROME_REFRESH_TOKEN: ${{ secrets.CHROME_REFRESH_TOKEN }}
```

### Update a Firefox listed build with reviewer notes

```yaml
- uses: ninoseki/go-webext-action@v1
  with:
    command: update
    browser: firefox
    file: ./dist/firefox.zip
    source: ./dist/source.zip
    channel: listed
    approval-notes: |
      Build with: docker run --rm -v $(pwd):/src example/build
  env:
    FIREFOX_CLIENT_ID: ${{ secrets.FIREFOX_CLIENT_ID }}
    FIREFOX_CLIENT_SECRET: ${{ secrets.FIREFOX_CLIENT_SECRET }}
```

### Sign a Firefox unlisted build

```yaml
- uses: ninoseki/go-webext-action@v1
  with:
    command: sign
    browser: firefox
    file: ./dist/firefox.zip
    source: ./dist/source.zip
    output: ./dist/firefox.xpi
  env:
    FIREFOX_CLIENT_ID: ${{ secrets.FIREFOX_CLIENT_ID }}
    FIREFOX_CLIENT_SECRET: ${{ secrets.FIREFOX_CLIENT_SECRET }}
```

### Update an Edge product

```yaml
- uses: ninoseki/go-webext-action@v1
  with:
    command: update
    browser: edge
    app-id: ${{ vars.EDGE_PRODUCT_ID }}
    file: ./dist/edge.zip
  env:
    EDGE_CLIENT_ID: ${{ secrets.EDGE_CLIENT_ID }}
    EDGE_API_KEY: ${{ secrets.EDGE_API_KEY }}
    EDGE_API_VERSION: v1.1
```

### Pin a specific go-webext version

```yaml
- uses: ninoseki/go-webext-action@v1
  with:
    version: v0.4.1
    command: status
    browser: chrome
    app-id: ${{ vars.CHROME_ITEM_ID }}
  env:
    CHROME_CLIENT_ID: ${{ secrets.CHROME_CLIENT_ID }}
    CHROME_CLIENT_SECRET: ${{ secrets.CHROME_CLIENT_SECRET }}
    CHROME_REFRESH_TOKEN: ${{ secrets.CHROME_REFRESH_TOKEN }}
```

## License

[MIT](LICENSE)
