# Bootstrap a TypeScript project

Explicit steps to bootstrap a typescript project with no hidden magic. Be aware
of each step. Only do it if you want it. Expand on these steps as you want.

## Getting started

Either create git repository and clone

    git clone git@github.com:ianhomer/my-repository.git
    cd my-repository

or initialise git repository locally and push

    mkdir my-repository && cd my-repository
    git init --initial-branch=main

Copy the following files into your repository as baseline files

- `.github/workflows/build.yaml`
- `.eslintc.js`
- `.gitinore`
- `.lintstagedrc`
- `src/index.ts` - typescript file to get rolling

Use the command line to copy files into place if you like

    export BRANCH_URI=ianhomer/bootstrap-typescript/main
    export RAW_URI=https://raw.githubusercontent.com/$BRANCH_URI
    mkdir -p .github/workflows src
    curl $RAW_URI/.github/workflows/build.yaml -sS \
      -o .github/workflows/build.yaml
    curl $RAW_URI/.eslintrc.json -sSO
    curl $RAW_URI/.gitignore -sSO
    curl $RAW_URI/.prettierignore -sSO
    curl $RAW_URI/.lintstagedrc -sSO
    curl $RAW_URI/tsconfig.json -sSO
    curl $RAW_URI/src/index.ts -sS -o src/index.ts
    curl $RAW_URI/src/my.test.ts -sS -o src/my.test.ts

Initialise yarn with typescript

    pnpm init
    pnpm add -D typescript ts-node @types/node

Review `package.json` and clean out entries not needed.

Add prettier, eslint, and format-package for linting

    pnpm add -D                        \
      prettier format-package          \
      eslint                           \
      eslint-config-prettier           \
      eslint-plugin-no-only-tests      \
      eslint-plugin-simple-import-sort \
      eslint-plugin-sonarjs            \
      @typescript-eslint/eslint-plugin \
      @typescript-eslint/parser

Add jest for unit testing

    pnpm add -D jest ts-jest @types/jest @jest/types
    pnpm ts-jest config:init

Add the following scripts to `package.json`

    "scripts": {
      "eslint": "eslint src --ext .ts",
      "eslint:fix": "eslint src --ext .ts --fix",
      "lint": "pnpm prettier && pnpm eslint",
      "lint:fix": "pnpm package:fix && pnpm prettier:fix && pnpm eslint:fix",
      "package:fix": "format-package -w",
      "prepare": "husky install",
      "prettier": "pnpx prettier --check .",
      "prettier:fix": "pnpx prettier --write .",
      "start": "ts-node src/index.ts",
      "test": "pnpm jest",
      "test:watch": "pnpm jest --watch"
    },

Check linting added OK and unit tests running

    pnpm lint:fix
    pnpm test

Lint on commit

    pnpm add -D husky lint-staged
    pnpm prepare
    pnpx husky add .husky/pre-commit "pnpx lint-staged"

Kick off `README.md` and add `## tl;dr` section guiding user what to do once
they've cloned the repository, start it off with something like

    touch README.md
    echo -e "# title\n\n## tl;dr\n\n    pnpm install" >> README.md

and add to this README as the repository evolves.

If this a public repository add a LICENSE file, for example BSD

    curl $RAW_URI/LICENSE -sSO

Commit and push

    git add .
    git commit -am "Start"

If you initialised git repository repository locally then add the remote
origin

    git remote add origin git@github.com:ianhomer/boot1.git

And push

    git push
