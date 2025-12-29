# gh-setup

Setup asset of Github Releases.

[![build](https://github.com/step-security/gh-setup/actions/workflows/ci.yml/badge.svg)](https://github.com/step-security/gh-setup/actions/workflows/ci.yml)

Key features of `gh-setup` are:

- **Works as a GitHub CLI extension (or a standalone CLI) as well as a GitHub Action.**
- **Could be used as a part to create a GitHub Action like `setup-*`.**

## As a GitHub Action

### Usage

``` yaml
# .github/workflows/doc.yml
[...]
    steps:
      -
        name: Setup tbls
        uses: step-security/gh-setup@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          repo: k1LoW/tbls
        # version: v1.60.0
        # os: linux
        # arch: amd64
        # bin-match: tbls
        # checksum: f1ee97bbf22d5324ec2b468d83f43088d9e5c61deb77fafc220b297e03d47574
        # force: true
        # strict: true
        # verify-attestation: true
        # attestation-flags: "--owner=k1LoW"
        # gh-setup-version: latest
      -
        name: Run tbls
        run: tbls doc
```

## As a part to create a GitHub Action like `setup-*`

``` yaml
# action.yml
name: 'Setup tbls'
description: 'GitHub Action for tbls, a CI-Friendly tool for document a database, written in Go.'
branding:
  icon: 'box'
  color: 'blue'
inputs:
  github-token:
    description: The GitHub token
    default: ${{ github.token }}
    required: false
  version:
    description: Version of tbls
    default: latest
    required: false
  force:
    description: Enable force setup
    default: ''
    required: false
  checksum:
    description: Checksum of tbls
    default: ''
    required: false
runs:
  using: 'composite'
  steps:
    -
      uses: step-security/gh-setup@v1
      with:
        repo: github.com/k1LoW/tbls
        github-token: ${{ inputs.github-token }}
        version: ${{ inputs.version }}
        bin-match: tbls
        checksum: ${{ inputs.checksum }}
        force: ${{ inputs.force }}
```

## Attestation Verification

In GitHub Actions:

```yaml
- name: Setup k1LoW/tbls
  uses: step-security/gh-setup@v1
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
    repo: k1LoW/tbls
    verify-attestation: true
    attestation-flags: "--owner=k1LoW"
```

