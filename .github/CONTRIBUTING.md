# Contributing

First of all, thanks for your interest in contributing to the `web-app`! 🎉

PRs are the preferred way to spike ideas and address issues, if you have time. If you plan on contributing frequently, please feel free to ask to become a maintainer, the more the merrier. 🤙

## Technical overview

This project uses following libraries for development:

- [typescript](http://www.typescriptlang.org/) for typed JavaScript

  <details>
  Every package is written in TypeScript. Only scripts and tool configs are in vanilla JS (but also typed via `//@ts-check`).

  We use [path aliases/mappings](https://www.typescriptlang.org/docs/handbook/module-resolution.html#path-mapping) for development (see `./tsconfig.json`)

  > **Why?**
  >
  > Otherwise we would have to run tsc watch all the time, to build physical ambient definitions. With alias you get immediate types updates as you type 👌.

  For production we use [project references/composite projects](https://www.typescriptlang.org/docs/handbook/project-references.html) for building type declarations per package (see `./tsconfig.build.json`)

  > **Why?**
  >
  > Every build package which is shipped to npm needs to contain type definitions. Project references allow us to build those type by running one command from the root. This technique is also very fast on consecutive rebuilds as it's cached under the hood via `"incremental": true` flag.

   </details>

- [babel](https://babeljs.io/) for transpilation

  <details>
   Babel is used to build es2015 vanilla JS from TypeScript.

  > **Why**
  >
  > While TypeScript is a transpiler itself, it is quite limited in terms of optimizing polyfills bundling and lacking lot's of transformation plugins/compilers like babel provides. Babel is an obvious choice and industry standard.

  </details>

- [emotion](https://emotion.sh/) for authoring css in js

  <details>

  > **Why**
  >
  > Emotion is a library designed for writing css styles with JavaScript. It provides powerful and predictable style composition in addition to a great developer experience with features such as source maps, labels, and testing utilities. Both string and object styles are supported.

  </details>

- [prettier](https://prettier.io/) for formatting
- [eslint](https://eslint.org/) for javascript/TypeScript linting

  <details>
   ESlint parser understands TypeScript thanks to https://github.com/typescript-eslint.

  - We use various plugins and common set of lint rules. Check `.eslintrc.js` for more details

  > **Why**
  >
  > TSlint is deprecated and won't be maintained soon. Also TypeScript team announced that moving forward, the official linter for TypeScript is gonna be eslint. (Btw did you know that company behind TSLint is an evil monster? Yup, Palantir, Google it for yoursel...)

  </details>

- [stylelint](https://stylelint.io/) for linting css
- [jest](https://jestjs.io/) for unit testing
  <details>
    Standard jest setup. We are not using `ts-jest` so tests are not type checked on build.
  </details>
- [cypress](https://docs.cypress.io/) for E2E and regression testing
- [yarn](https://yarnpkg.com/lang/en/) for package management
- [yarn scripts](https://yarnpkg.com/lang/en/docs/cli/run/) for executing tasks
  <details>
  yarn scripts are grouped by `:` -> `build:js`, `build:ts` -> `build` will run (most of the time) task form the group
  </details>
- [storybook](https://storybook.js.org/) for developing components in isolation (custom component catalogue)
- [jenkins](https://ci.twisto.wtf/blue/organizations/jenkins/Frontend/activity) for CI
- [Apollo](https://www.apollographql.com/docs/apollo-server/) `client` for work with GraphQL, but also `server` for testing - mocking GraphQL

### Folder structure

```
├── .storybook/
        (all things related to storybook setup)
├── config/
        (all things related to configuring build/tools setup, etc)
├── cypress/
        (all things related to E2E and regression tests)
├── custom-types/
        (custom/overrides 3rd party type definitions)
├── src/
        (all src/apps reside here)
├── scripts/
        (custom scripts, which are executed from npm scripts)
```

## Getting started

### Creating a Pull Request

If you've never submitted a Pull request before please visit http://makeapullrequest.com/ to learn everything you need to know.

### Setup

> **Prerequisites**
>
> - git
> - nodeJs LTS
> - yarn (`npm i -g yarn`)
> - editor (VSCode is preferred and supported. We also ship recommended extensions, so you should get prompt to install those on initial project opening)

1.  clone this repo `git clone`
1.  clone [TwistoPayments/tripping-avenger](https://github.com/twistopayments/tripping-avenger) repo `git clone`
1.  Make a `git checkout -b branch-name` branch for your change. Branch should include your name prefix. (example - John Doe should create `jd-do-some-fixes`)
1.  Run `yarn install`
1.  Setup [Lokalise](https://github.com/TwistoPayments/web-app/blob/master/docs/lokalise.md)
1.  Run `yarn build:locale` to generate translation files
1.  Now your environment is ready. explore npm-scripts within root `package.json` to execute what you need (tests, dev-server, storybook). Few examples:

    - run tests `yarn test`
    - run storybook `yarn start:storybook`

### Development

1. Pick task from [Trello](https://trello.com/b/4aVeN5VC/fe-web-app) Shelf or Backlog and start working on it
1. Create new branch with your name prefix `mh-description-of-your-task` ( `mh` 👉 martin hochel)
1. Make sure to add unit test or story (if it makes sense)
1. If there is a `*.spec.ts` file, update it to include a test for your change, if needed. If this file doesn't exist, please create it.
1. Make sure to have all status checks on GitHub green before requesting review
1. Make sure to have translations ready via `lokalise` before merging. Checkout localization [docs](../docs/lokalise.md)
1. Decide whether your PR should be tested by QA team, use corresponding `QA:skipped` or `QA:required` tag and optionally create testserver for your branch by with "Run" button in [CI](https://ci.twisto.wtf/blue/organizations/jenkins/Testserver/activity/)

#### FAQ:

- I wanna update my branch against latest master. How to do it
  - 👉 rebase. to prevent nasty rebase conflicts, squash your branch before rebasing against master
    `git rebase -i master`
- My work is done (Approved PR). Now what?
  - 👉 if you haven't squashed your work on your machine, use **SQUASH AND MERGE**
  - 👉 if your contains multiple atomic commits, use **REBASE AND MERGE**

#### 🧪 Tests

Test are written and run via Jest (in watch mode) 💪

```sh
# Run whole test suite in watch mode
yarn test

# Get code coverage
yarn test:coverage
```

##### Unit/Integration Tests

We're using [react-testing library](https://testing-library.com/docs/@testing-library/react/intro) for mostly integration test on component level.

Please read following articles, which reflect how we write tests:

- https://kentcdodds.com/blog/why-i-never-use-shallow-rendering
- https://kentcdodds.com/blog/testing-implementation-details
- https://kentcdodds.com/blog/common-testing-mistakes
- https://kentcdodds.com/blog/how-to-know-what-to-test

##### E2E tests

We are using [cypress](https://docs.cypress.io) for visual regression and E2E testing. For visual regression tests there is [Loki](https://loki.js.org/) plugin used in cypress.

For more visit our [docs 📂](../docs/e2e-regression-tests.md)

```sh
# Open cypress E2E
yarn cy:open
```

```sh
# Update visual test files on your base branch
yarn visual:update

# Run the tests on your feature branch
yarn visual:test

# Loki compares views in storybook files, so it may be necessary to add storybook file for the specific thing that you want tested
```

#### 💅 Style guides

Style guides are enforced by robots _(I meant prettier and eslint of course 🤖 )_, so they'll let you know if you screwed something, but most of the time, they'll auto-fix things for you. Magic right?

```sh
# Check linting in all packages (lint ts, lint css, check formating)
yarn lint

# Check lint in typescript
yarn lint:ts
# fix lint issues in typescript
yarn lint:ts --fix

# check lint in css
yarn lint:css
# fix lint issues in typescript
yarn lint:fix

# check formating
yarn lint:format
# apply formating
yarn format
```

#### Commit conventions (via commitizen)

- this is preferred way how to create conventional-changelog valid commits
- if you prefer your custom tool we provide a commit hook linter which will error out, it you provide invalid commit message
- if you are in rush and just wanna skip commit message validation pass the `--no-verify` (`-n`) flag to git when committing (e.g. `git commit -n -m "wip"`).
  > If you do this please squash your work when you're done with proper commit message so standard-version can create Changelog and bump version of your library appropriately)

```sh
# invoke [commitizen CLI](https://github.com/commitizen/cz-cli)
yarn commit
```

## License

By contributing your code to the {library-name} GitHub Repository, you agree to license your contribution under the MIT license.
