# WordPress Coding Standards Docs

## About

This repo contains the source for the [WordPress Coding Standards](https://developer.wordpress.org/coding-standards/) published at [https://developer.wordpress.org/coding-standards](https://developer.wordpress.org/coding-standards).

When creating new files, these must be added to the [manifest.json](https://github.com/WordPress/wpcs-docs/blob/master/manifest.json) file.

Removing files also requires updating the [manifest.json](https://github.com/WordPress/wpcs-docs/blob/master/manifest.json) file. After deletion and sync with DevHub, the page also needs to be manually deleted from DevHub.

## Linting

Basic developer tooling has been added that enables Markdown linting using [markdownlint](https://github.com/DavidAnson/markdownlint) to ensure the markdown style used in this repo is compatible with the markdown parser used for publishing this repo to the developer hub.

The [markdownlint-cli configuration](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-scripts/#lint-md-docs) is inherited from the [@wordpress/scripts](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-scripts/#lint-md-docs) package, this results in a straight forward zero-config setup in this repo.

### Install

The ease the maintenance burden of the linting setup in this repo it use's Node.js' `lts/*`, which at the time of writing was Node.js v16.x and npm v8.x, further details on the latest Node.js LTS release can be read at [https://nodejs.org/en/about/releases/](https://nodejs.org/en/about/releases/), this should result in relatively little maintenance of this setup.

To install and run markdownlint:

```shell
npm install
npm test
```
