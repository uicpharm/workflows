# Reusable Workflows for UIC Pharmacy Projects

This repository contains [reusable workflows][gh-workflows] and
[composite actions][gh-actions] for GitHub Actions in UIC Pharmacy projects. By using
these, we reduce repeated code and make our workflows more readable.

[gh-workflows]: https://docs.github.com/en/actions/using-workflows/reusing-workflows
[gh-actions]: https://docs.github.com/en/actions/creating-actions/creating-a-composite-action

## Composite Actions

Composite actions are used under a job's `steps` section.

### `build-and-publish-docker`

A composite action to help make building your project and publishing a Docker image to
[ghcr.io](https://ghcr.io) a more concise process.

[Read more](./actions/build-and-publish-docker/README.md)

### `setup-and-install`

Sets up a Node.js environment and installs your project's dependencies.

[Read more](./actions//setup-and-install/README.md)

## Reusable Workflows

Reusable workflows can be used as an entire job in the `jobs` section of your workflow.

### `build.yml`

Just checks out, installs, and builds the project. Useful for a test workflow to make sure
a project builds successfully.

Called as: `uicpharm/workflows/.github/workflows/build.yml@v1`

### `check-version.yml`

Checks that the version in your project's `package.json` file matches the tagged version.
Intended to be used when pushing a tagged version, and is useful as a precautionary step
before publishing a Docker image.

Called as: `uicpharm/workflows/.github/workflows/check-version.yml@v1`

### `context-dump.yml`

Dumps all existing contexts to the logs. Just for debugging purposes.

Called as: `uicpharm/workflows/.github/workflows/context-dump.yml@v1`

### `lint-and-test.yml`

Checks out your project, installs dependencies, and runs `test` and `standards` scripts,
if they exist.

Called as: `uicpharm/workflows/.github/workflows/lint-and-test.yml@v1`
