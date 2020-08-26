# Rust Private Crate Template :lock: :crab:
Write small, focused libraries - one crate at a time :package:

## How to add as a dependency
- Ensure your `cargo` installation is configured for [Git Authentication]
- In your `Cargo.toml`, add a section like so: `crate_name = { git = "https://github.com/strongmind/crate_name", branch = "v1.0.0" }`


## How to maintain
If you choose to use the [CICD process](#cicd-process) bundled with this template, you'll need to follow [Conventional Commits].

Conventional Commits outlines commit message prefixes like `feat!`, `feat`, `fix`, `chore`.

These translate to semver bumps: `MAJOR`, `MINOR`, `PATCH`, and none.

If you choose to use the [Squash merge strategy] when merging Pull Requests, make sure the merge commit title follows these guidelines.

### CICD Jobs

#### Setup
Add a [Secret] to the repository with a personal [Github Personal Access Token] tied to a repo administrator's account.
<details>
  <summary>Why? Click to expand</summary>
  
  > Github Actions can open PRs, but those PRs will not trigger other Actions (`ci_build` or `publish`).
  > Using a PAT allows the repo to check that the PR is mergeable, and run the `publish` job on merge.

</details>

#### `ci_build`
When you open a PR, a job named `ci_build` will:
- run `cargo build` against the PR
- run `cargo test` against the PR

**It is recommended that you make this a [required status check] for PRs targeting `main`.**

#### `bump`
When you merge a PR, a job named `bump` will:
1. decide based on the commit message the next version number (if any)
1. if a bump is to be made:
    1. update the version in `Cargo.toml` and `Cargo.lock.toml`
    1. open a Pull Request to `main` with the code changes

#### `publish`
When you merge a PR created by `bump`, a job named `publish` will:
1. create a branch named `v{{VERSION_NUMBER}}`
1. create a tag named `v{{VERSION_NUMBER}}`
1. push a snapshot of your `main` branch to the new version branch
1. run `cargo +nightly doc` to generate documentation for your crate
1. push the built docs to a `github-pages` branch

### Enabling docs hosted by Github Pages
**note: this is potentially dangerous, as Github Pages are publicly hosted and cannot be secured by an identity provider**
1. Go to repo settings
1. scroll down to github pages
1. set `source` to `github-pages`
1. it should "just work" :tm:

[Git Authentication]: https://doc.rust-lang.org/cargo/appendix/git-authentication.html#git-authentication
[required status check]: https://docs.github.com/en/github/administering-a-repository/enabling-required-status-checks
[Squash merge strategy]: https://docs.github.com/en/github/administering-a-repository/about-merge-methods-on-github
[Conventional Commits]: https://www.conventionalcommits.org/en/v1.0.0/
[Secret]: https://docs.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets
[Github personal access token]: https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token


