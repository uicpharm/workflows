# Reusable Workflows for UIC Pharmacy Projects

This repository contains [reusable workflows][gh-workflows] and
[composite actions][gh-actions] for GitHub Actions in UIC Pharmacy projects. By using
these, we reduce repeated code and make our workflows more readable.

[gh-workflows]: https://docs.github.com/en/actions/using-workflows/reusing-workflows
[gh-actions]: https://docs.github.com/en/actions/creating-actions/creating-a-composite-action

## Composite Actions

Composite actions are used under a job's `steps` section.

### `build-and-publish-docker`

Will use the project's `Dockerfile` to build an image and publish it to
[ghcr.io](https://ghcr.io), tagging the image with `latest` and the version of your
project according to its `package.json` version.

#### Example

Called as: `uicpharm/workflows/actions/build-and-publish-docker@v1`

```yml
steps:
   - uses: actions/checkout@v4
   - uses: uicpharm/workflows/actions/setup-and-install@v1
   - run: npm run build
   - uses: uicpharm/workflows/actions/build-and-publish-docker@v1
```

#### Notes

When tagging the version, it expects [semver](https://semver.org) and will tag the image
for every level of the version.

For instance, if your package version is `1.2.3`, it will provide these version tags:

   - `1`
   - `1.2`
   - `1.2.3`

It also detects prerelease values, such as `1.2.3-beta.4`, and will tag _only the
prerelease version in that case. That way, prerelease versions don't pollute the tags.

This version tagging method is a standard practice with images so that we can, for
instance, point to just a specific major version, and always get the latest instance of
that major version. For instance, `myproject:1` will always get the latest version of the
v1.x line.

### `setup-and-install`

Will set up a Node.js environment based on your project's `.nvmrc` file, and then runs
`npm ci` to install dependencies.

#### Example

Called as: `uicpharm/workflows/actions/setup-and-install@v1`

```yml
steps:
   - uses: actions/checkout@v4
   - uses: uicpharm/workflows/actions/setup-and-install@v1
   - run: npm run build
```

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
