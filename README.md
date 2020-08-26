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

### Setup

#### Adding your code
1. Click "Use this template" to fork it to your new library, e.g. named `lib_name`.
1. In your new library, run `cargo new --lib lib_name`
1. This will create a shell project that can be pushed to this repo

#### CICD Setup
Add a [Secret] named `GITHUB_PAT` to the repository with a [Github Personal Access Token] tied to a repo administrator's account.
<details>
  <summary>Why? Click to expand</summary>
  
  > Github Actions can open PRs, but those PRs will not trigger any required checks (e.g. `ci_build`).
  > Using a PAT allows the repo to check that the PR opened by [`bump`](#bump) is mergeable.

</details>

### CICD Jobs

#### `ci_build`
**When you open a PR**, this job will:
- run `cargo build` against the PR
- run `cargo test` against the PR

**It is recommended that you make this a [required status check] for PRs targeting `main`.**

#### `bump`
**When you merge a PR**, this job will:
1. decide the next version number based on the commit message
    - _(no version will be created if the commit contains `[NO_PUB]`, `ci:` or `bump:`)_
1. if a bump is to be made:
    1. update the version in `Cargo.toml` and `Cargo.lock.toml`
    1. open a Pull Request to `main` with the code changes
    1. create and push a tag for the new version (e.g. `v1.0.0`)

#### `create_github_release`
**When a tag is pushed to `main`** (e.g. by `bump`), this job will:
1. As a housekeeping step, this creates a Github release based on the new tag

#### `publish_ver`
**When a tag is pushed to `main` (e.g., by `bump`)**, this job will:
1. create a branch named the same as the tag (e.g. `v1.0.0`)
1. push a snapshot of your `main` branch to the new version branch

#### `publish_docs`
**When a tag is pushed to `main` (e.g., by `bump`)**, this job will:
1. run `cargo +nightly doc` to generate documentation for your crate
1. push the built docs to the `docs` branch

### Enabling docs hosted by Github Pages
**NOTE: Even for private repositories, Github Pages are publicly hosted and cannot be secured by an identity provider**
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
