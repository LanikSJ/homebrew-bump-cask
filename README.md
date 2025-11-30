# Homebrew Bump Cask GitHub Action

A GitHub Action that automates updating Homebrew casks by wrapping the
`brew bump-cask-pr` command. It simplifies keeping casks up-to-date with new
project releases and supports both manual updates and scheduled automated
checking for outdated casks.

Works on Ubuntu and macOS runners.

## 📑 Table of Contents

- [🐙 Security Note](#-security-note)
- [💡 Usage](#-usage)
  - [⚙️ Standard mode](#standard-mode)
  - [🔍 Livecheck mode](#-livecheck-mode)
- [🔧 Troubleshooting](#-troubleshooting)
- [📚 Examples](#-examples)
- [🤝 Contributing](#-contributing)
- [📄 License](#-license)

## 🔒 Security Note

This action creates pull requests to Homebrew taps, which requires forking
repositories and writing code changes. The provided GitHub token must have
appropriate permissions to create forks and pull requests. Never use the default
`GITHUB_TOKEN` unless you have `public_repo` permissions enabled on
your repository.

## 💡 Usage

One should use the [Personal Access
Token](https://github.com/settings/tokens/new?scopes=public_repo,workflow)
for `token` input to this Action, not the default `GITHUB_TOKEN`, because
`brew bump-cask-pr` creates a fork of the cask's tap repository (if needed) and
then creates a pull request.

> There are two ways to use this Action.

### Standard Mode

Use if you want to simply bump the cask, when a new release happens.

Listen for new tags in workflow:

```yaml
on:
  # trigger when release got created (preferred)
  release:
    types: [released]
  # trigger on tag push
  # push:
  #   tags:
  #     - "*"
```

The Action will automatically extract all needed information; you just need to
specify the following step in your workflow:

```yaml
- name: Update Homebrew cask
  uses: LanikSJ/homebrew-bump-cask@v1
  with:
    # **Required:** GitHub access token with 'public_repo' and 'workflow' scopes
    token: ${{secrets.TOKEN}}
    # *Optional:* Custom commit user name (defaults to the token owner)
    user_name: name
    # *Optional:* Custom commit user email (defaults to the token owner)
    user_email: email@example.com
    # *Optional:* Organization to create tap repo fork in
    org: ORG
    # *Optional:* Use the origin repository instead of forking
    # (for maintainers with write access)
    no_fork: false
    # *Optional:* Tap to check for outdated casks (use instead of cask list)
    tap: USER/REPO
    # **Required:** Name of the cask to bump
    cask: cask
    # *Optional:* Specific tag/version to bump to
    # (auto-detected from release/tag if not set)
    tag: ${{github.ref}}
    # *Optional:* Git SHA of the version to bump to
    # (auto-detected from release if not set)
    revision: ${{github.sha}}
    # *Optional:* Force bump even if PR already exists
    # (set to true to skip duplicate checks)
    force: false
```

### 🔍 Livecheck Mode

If `livecheck` input is set to `true`, the Action will run `brew livecheck` to
check if any provided casks are outdated or if tap contains any outdated casks
and then will run `brew bump-cask-pr` on each of those casks with proper
arguments to bump them.

Might be a good idea to run this on schedule in your tap repo, so one gets
automated PRs updating outdated casks.

If there are no outdated casks, the Action will just exit.

```yaml
- name: Update Homebrew cask
  uses: LanikSJ/homebrew-bump-cask@v1
  with:
    # **Required:** GitHub personal access token with 'public_repo' scope
    token: ${{secrets.CUSTOM_PERSONAL_ACCESS_TOKEN}}
    # *Optional:* Custom commit user name (defaults to the token owner)
    user_name: user_name
    # *Optional:* Custom commit user email (defaults to the token owner)
    user_email: email@example.com
    # *Optional:* Organization to create tap repo fork in
    org: ORG
    # *Optional:* Homebrew tap to check for outdated casks
    # (use instead of cask list)
    tap: USER/REPO
    # *Optional:* Specific casks to check (comma-separated if multiple)
    cask: cask-1, cask-2, cask-3, ...
    # *Optional:* Force bump even if PR already exists
    # (set to true to skip duplicate checks)
    force: false
    # **Required for livecheck mode:** Enables livecheck functionality
    livecheck: true
```

If only the `tap` input is provided, all casks in the given tap will be
checked and bumped if needed.

## 🔧 Troubleshooting

### Common Issues

- **Action fails with authentication error**: Ensure your GitHub token has the
  required scopes (`public_repo` and `workflow` for standard mode). Verify the
  token is stored as a repository secret and referenced correctly.

- **Pull request not created**: Check that the token has permissions to create
  forks and pull requests for the target repository. For private taps, ensure
  the token owner has the necessary access.

- **No outdated casks found in livecheck mode**: Verify that the correct tap is
  specified and that the casks actually have newer versions available that
  `brew livecheck` can detect.

- **Permission denied**: If using `no_fork: true`, ensure the token has write
  access to the target repository.

### Debugging

Enable debug logging by setting `ACTIONS_STEP_DEBUG` to `true` in your
repository settings or as a secret.

## 📚 Examples

- <https://github.com/LanikSJ/homebrew-tap/blob/main/.github/workflows/bump-casks.yml>

## 🤝 Contributing

This project welcomes contributions! Please see the repository guidelines for:

- Reporting bugs or requesting features via
  [GitHub Issues](https://github.com/LanikSJ/action-homebrew-bump-cask/issues)
- Submitting pull requests with improvements or fixes
- Best practices for development and testing

## 📄 License

This project is licensed under the MIT License - see the
[LICENSE](LICENSE) file for details.
