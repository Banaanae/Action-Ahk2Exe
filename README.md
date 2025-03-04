# Action-Ahk2Exe

A Github actions script gets the compiler files from github repositories instead autohotkey.com to bypass cloudflare DDoS protection.

## Inputs

| Input   | Type             | Required | Description                                                       |
|---------|------------------|----------|-------------------------------------------------------------------|
| in      | String           | Yes      | Name (and path if not root) of file to compile                    |
| out     | String           | No       | Name (and path if not root) to give the compiled script           |
| version | String           | No       | Version to compile with, as a Github tag                          |
| bits    | String or Number | No       | Compiler bit count (also specify text defining encoding, v1 only) |

## Remarks

While this actions should work with v1.1 scripts, it should be noted that really old versions of AutoHotkey will not work, as their directory structure in releases is different

The compiler can only convert file in each step, this will change in the future

Finally, this action will only run on `windows-latest`

## Example usage

Build script and upload to releases
```yaml
name: Build Binary and release

on: # Only run when a release is created
  push:
    tags:
      - '*'

permissions:
  contents: write

jobs:
  BuildAndRelease:
    name: Build and Release
    runs-on: windows-latest
    steps:
      - name: Checkout # Get repository
        uses: actions/checkout@v2

      - name: Ahk2Exe # Build binary
        id: ahk2exe
        uses: Banaanae/Action-Ahk2Exe@main
        with:
            in: MyScript.ahk
          
      - name: Release # Upload binary to most recent release
        uses: softprops/action-gh-release@v2
        with:
          files: MyScript.exe
```
