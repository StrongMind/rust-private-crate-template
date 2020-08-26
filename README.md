# Private Rust Crate Template

## How to add as a dependency
- Ensure your `cargo` installation is configured for [Git Authentication]
- In your `Cargo.toml`, add a section like so: `crate_name = { git = "https://github.com/strongmind/crate_name", branch = "v1.0.0" }`


## How to maintain
If you choose to use the [CICD process](#cicd-process) bundled with this template, you'll need to follow [Conventional Commits].

Conventional Commits outlines commit message prefixes like `feat!`, `feat`, `fix`, `chore`.

These translate to semver bumps: `MAJOR`, `MINOR`, `PATCH`, and none.

If you choose to use the [Squash merge strategy] when merging Pull Requests, make sure the merge commit title follows these guidelines.

### CICD process

#### `ci_build`
When you open a PR, a job named `ci_build` will:
- run `cargo build` against the PR
- run `cargo test` against the PR

**It is recommended that you make this a [required status check] for PRs targeting `main`.**

#### `publish`
When you merge a PR, a job named `publish` will:
1. decide based on the commit message the next version number (if any)
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
