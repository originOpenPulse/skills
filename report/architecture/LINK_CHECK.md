# Link Check

专题首页：[Big Tech Architecture Atlas](README.md)

## Manual Link Check

Run this from the repository root:

```powershell
$files = Get-ChildItem . -Recurse -Filter *.md
$issues = @()
foreach ($file in $files) {
  $content = Get-Content -Raw -LiteralPath $file.FullName
  $matches = [regex]::Matches($content, '\[[^\]]+\]\(([^)]+)\)')
  foreach ($m in $matches) {
    $target = $m.Groups[1].Value
    if ($target -match '^(https?:|mailto:|#)') { continue }
    $path = ($target -split '#')[0]
    if ([string]::IsNullOrWhiteSpace($path)) { continue }
    $resolved = Join-Path $file.DirectoryName $path
    if (-not (Test-Path -LiteralPath $resolved)) {
      $issues += [PSCustomObject]@{ File = $file.FullName; Target = $target }
    }
  }
}
$issues
```

## Suggested GitHub Action

If this project becomes a standalone repository, add a link checker workflow:

```yaml
name: Link Check

on:
  schedule:
    - cron: "0 3 * * 1"
  workflow_dispatch:
  pull_request:
    paths:
      - "**/*.md"

jobs:
  lychee:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: lycheeverse/lychee-action@v2
        with:
          args: --verbose --no-progress './**/*.md'
```

## Policy

- Broken internal links should be fixed immediately.
- Broken external demo links can be replaced with a more stable repository.
- Prefer repository root links over deep paths when deep paths are unstable.

