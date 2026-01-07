# `get-package-info`

Parses a package.json file and returns the fields you specify. If you refer to an object,
it will return the object as stringified JSON. If you want to refer to a key in an object,
you can use dot notation, like `repository.type`, and it will set a variable with the name
of the key with the dots converted to underscores, like `repository_type`. If a key does
not exist, it will return an empty variable.

Only official values documented by the [package.json spec][package-spec] will be returned.
If you want to retrieve a custom field, you have two options:

   - The entire JSON is returned as the variable `json`. You could parse the entire JSON
     yourself and retrieve the desired field(s).
   - You can request one custom field at a time, and the custom field name will be stored
     in `custom_field` and its value in `custom_value`.

[package-spec]: https://docs.npmjs.com/cli/v11/configuring-npm/package-json

## Optional inputs

   - `path` - The path of the directory containing the package.json file you want to parse
     if you have multiple package.json files. Defaults to root directory of your project.
   - `fields` - Space-separated list of fields to retrieve. If you need a key of an
     object, you can access it with dot notation like `repository.type`. By default, all
     standard package.json keys will be returned.

## Example

Called as: `uicpharm/workflows/actions/get-package-info@v1`

```yml
steps:
   # ...earlier steps...
   - uses: uicpharm/workflows/actions/get-package-info@v1
     id: pkg
     with:
         fields: name version
   # Example usage:
   - run: |
         echo "Project ${{steps.pkg.outputs.name}}, ${{steps.pkg.outputs.version}}"
```

## Non-existing keys

If a key doesn't exist, it will be returned as an empty value.

## Objects

If you ask for a key that is an object, such as the `scripts` or `dependencies` keys, it
will return stringified JSON of the entire object. You could then use jq to parse the JSON
any way you wanted.

Alternatively, you can specify just the key of an object. For instance, if you wanted the
`url` key inside of `bugs`, you can include `bugs.url` as one of the fields. It will set
the variable `bugs_url`, or fully accessible as `${{steps.YOUR_STEP_NAME.bugs_url}}`.
