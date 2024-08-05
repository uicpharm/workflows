# `setup-and-install`

Will set up a Node.js environment based on your project's `.nvmrc` file, and then run
`npm ci` to install dependencies.

## Example

Called as: `uicpharm/workflows/actions/setup-and-install@v1`

```yml
steps:
   - uses: actions/checkout@v4
   - uses: uicpharm/workflows/actions/setup-and-install@v1
   - run: npm run build
```
