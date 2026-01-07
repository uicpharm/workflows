# `parse-semver`

Will parse all of the components of a semver version string, without using any
dependencies. It uses the RegEx pattern recommended by the the Semantic Versioning website
FAQ: "[Is there a suggested regular expression (RegEx) to check a SemVer string?][faq]"

[faq]: https://semver.org/#is-there-a-suggested-regular-expression-regex-to-check-a-semver-string

## Output

   - `prefix`: Any leading non-numeric prefix, such as "v".
   - `full`: The full version string. Returns the original input if invalid.
   - `major`: The major version, i.e. `1` from `1.2.3`.
   - `minor`: The minor version, i.e. `2` from `1.2.3`.
   - `patch`: The patch version, i.e. `3` from `1.2.3`.
   - `prerelease`: The pre-release version, i.e. `beta.1` from `1.2.3-beta.1`.
   - `build`: The build version, i.e. `build.456` from `1.2.3+build.456` or
     `1.2.3-beta.1+build.456`
   - `valid`: Boolean of `true` or `false` whether the string is valid semver.

## Example

Called as: `uicpharm/workflows/actions/parse-semver@v1`

```yml
steps:
   # ...earlier steps...
   - uses: uicpharm/workflows/actions/parse-semver@v1
     id: semver
     with:
         version: 'v1.2.3'
   # Example usage:
   - run: |
         if ${{steps.semver.outputs.valid}}; then
            echo "The x.y are ${{steps.semver.outputs.major}}.${{steps.semver.outputs.minor}}"
            echo "Prefix: ${{steps.semver.outputs.prefix}}"
            echo "Full: ${{steps.semver.outputs.full}}"
            echo "Major: ${{steps.semver.outputs.major}}"
            echo "Minor: ${{steps.semver.outputs.minor}}"
            echo "Patch: ${{steps.semver.outputs.patch}}"
            echo "Prerelease: ${{steps.semver.outputs.prerelease}}"
            echo "Build: ${{steps.semver.outputs.build}}"
         else
            echo 'Oops, my version was not valid semver.'
         fi
```
