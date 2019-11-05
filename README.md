# Prettier with Linting - Personal Setup

A place to keep track of my favorite configuration of [Prettier](https://prettier.io/), integrated with [TSLint](https://palantir.github.io/tslint/)

## Table of Contents

- [Setup Prettier](#SetupPrettier)
- [Itegrate with TSLint](#ItegratewithTSLint)
- [Create Pre-Commit Hook](#CreatePre-CommitHook)

## Setup Prettier

### 1. Install Prettier as dev dependency:

```bash
npm install --save-dev --save-exact prettier
```

> "We recommend pinning an exact version of prettier in your package.json as we introduce stylistic changes in patch releases."

### 2. Add .prettierrc file:

Reference JSON settings in [.prettierrc](./.prettierrc)

### 3. Add npm script to run Prettier on entire project:

Under "Scripts" in package.json, add:

```json
"prettier": "prettier --write \"**/*.{js,jsx,vue,flow,ts,css,less,scss,html,json,graphql,md,gfm,mdx,yaml,yml}\""
```

> Glob pattern is setup to match with any filetype currently supported by Prettier (JavaScript, including ES2017, JSX, Angular, Vue, Flow, TypeScript, CSS, Less, and SCSS, HTML, JSON, GraphQL, Markdown, including GFM and MDX, YAML)

## Itegrate with TSLint

### 1. Install tslint-plugin-prettier and tslint-config-prettier as dev dependencies:

```bash
npm install --save-dev tslint-config-prettier tslint-plugin-prettier
```

### 2. In tslint.json, add:

```json
{
  "rulesDirectory": ["tslint-plugin-prettier", "tslint-config-prettier"],
  "rules": {
    "prettier": true
  }
}
```

> **tslint-config-prettier** is a config that disables rules that conflict with Prettier. Make sure to put it **last** in the rulesDirectory array, so it gets the chance to override other configs.

Also, update ordered-imports:

```json
  "ordered-imports": [
    true,
    {
      "grouped-imports": true
    }
  ],
```

See reference to full [tslint.json](./tslint.json)

### 3. Add npm script to run TSLint --fix on entire project:

Under "Scripts" in package.json, add:

```json
    "tslint": "tslint --project ./tsconfig.json",
    "tslint-fix": "tslint --fix --project ./tsconfig.json"
```

## Create Pre-Commit Hook

> "Make sure Prettier is installed and is in your devDependencies before you proceed."

### 1. Install husky and lint-staged as dev dependencies:

```bash
npm install --save-dev husky lint-staged
```

### 2. In package.json add:

```json
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.{ts}": [
      "prettier --write",
      "tslint --fix",
      "git add"
    ],
    "*.{js,jsx,vue,flow,css,less,scss,html,json,graphql,md,gfm,mdx,yaml,yml}": [
      "prettier --write",
      "git add"
    ]
  }
```

## License

[MIT](./LICENSE)
