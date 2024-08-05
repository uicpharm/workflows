# `build-and-publish-docker`

Uses the project's `Dockerfile` to build an image and publish it to
[ghcr.io](https://ghcr.io), tagging the image with `latest` and the version of your
project according to its `package.json` version.

## Optional inputs

   - `name` - Name to assign the Docker image (Default: repository name, including your
     organization, i.e. "uicpharm/sample-project"). Especially if you have multiple
     services that will be built into images in one repo, you may use this. For example:
     "uicpharm/proj/service1" and "uicpharm/proj/service2".
   - `file` - Docker file to use for the build (Default: `Dockerfile`).
   - `platforms` - Platforms to build the image for (Default: `linux/amd64,linux/arm64`,
     in other words, 64-bit Intel and Apple Silicon architectures).

## Example

Called as: `uicpharm/workflows/actions/build-and-publish-docker@v1`

```yml
steps:
   - uses: actions/checkout@v4
   - uses: uicpharm/workflows/actions/setup-and-install@v1
   - run: npm run build
   - uses: uicpharm/workflows/actions/build-and-publish-docker@v1
```

## Notes

When tagging the version, it expects [semver](https://semver.org) and will tag the image
for every level of the version.

For instance, if your package version is `1.2.3`, it will provide these version tags:

   - `1`
   - `1.2`
   - `1.2.3`

It also detects prerelease values, such as `1.2.3-beta.4`, and will tag _only_ the
prerelease version in that case. That way, prerelease versions don't pollute the tags.

This version tagging method is a standard practice with images so that we can, for
instance, point to just a specific major version, and always get the latest instance of
that major version. For instance, `myproject:1` will always get the latest version of the
v1.x line.
