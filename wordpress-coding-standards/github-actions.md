# GitHub Actions Workflow Standards

GitHub Actions workflows operate in a highly privileged software supply chain environment. Workflows can access repository secrets, push code, create releases, publish packages, and interact with external services. A security weakness in a workflow file can have severe consequences.

WordPress uses two complementary linting tools to help maintain the quality and security of workflow files in the `.github/workflows` directory: [Actionlint](https://github.com/rhysd/actionlint) and [Zizmor](https://github.com/zizmorcore/zizmor). This page documents the tools and how contributors should address errors or warnings that they report.

## Actionlint

Actionlint is a static checker for workflow files. It focuses primarily on correctness: syntax validation, type checking for expressions, validation of inputs for actions and reusable workflows, syntax checking of shell scripts, and other common mistakes. See the [Actionlint documentation](https://github.com/rhysd/actionlint) for details.

Actionlint runs on pull requests and on pushes to the main branches on [the wordpress-develop repo](https://github.com/WordPress/wordpress-develop). It reports its findings as check results, just like the unit test and coding standards workflows. A failing Actionlint check must be fixed before the changes in the PR can be committed.

## Zizmor

Zizmor is a security-focused linter for workflow files. It detects template injection vulnerabilities, excessive permissions, dangerous triggers, unpinned dependencies, credential persistence, and dozens of other security weaknesses. See the [Zizmor documentation](https://docs.zizmor.sh/audits/) for details.

Zizmor also runs on pull requests and on pushes to the main branches on [the wordpress-develop repo](https://github.com/WordPress/wordpress-develop) and reports its findings to [GitHub Code Scanning](https://docs.github.com/en/code-security/code-scanning/introduction-to-code-scanning/about-code-scanning). This means:

- Results are available on the Security → Code Scanning tab of the repo for users with administrative permission on the repo.
- Errors and warnings that are newly introduced in a pull request will cause the code scanning check to fail. A "Code scanning results" status check will report failures, with inline annotations on the affected file and line.
- Existing issues act as a baseline and won't affect new pull requests until they are fixed or dismissed.

## Running locally

If you're making changes to workflow files in the `.github/workflows` directory, you can run both linting tools locally before pushing. Actionlint and Zizmor are both available via package managers for all operating systems, as well as via Docker images.

- [Actionlint installation instructions](https://github.com/rhysd/actionlint/blob/main/docs/install.md)
- [Zizmor installation instructions](https://docs.zizmor.sh/installation/)

### Running Actionlint

From the root of the repository, run:

```
actionlint
```

### Running Zizmor

From the root of the repository, run (note the trailing period):

```
zizmor .
```

To enable the online audits that check for known-vulnerable actions and impostor commits, provide a GitHub token:

```
GH_TOKEN=$(gh auth token) zizmor .
```

Some findings that are reported locally may be suppressed in the repository's Code Scanning settings. If you encounter a locally reported finding that does not appear in Code Scanning results, check whether it has been dismissed.

## Addressing common security issues

The following sections cover common findings and how to address them.

For full information about specific results from the linting tools, consult [the Actionlint documentation](https://github.com/rhysd/actionlint/blob/main/docs/checks.md) and [the Zizmor documentation](https://docs.zizmor.sh/audits/).

### Template injection

Template injection occurs when a GitHub Actions expression such as `${{ github.event.issue.title }}` is used directly within a `run:` block. GitHub Actions expressions are interpreted _prior_ to running the script, therefore an attacker who controls the expression value can inject arbitrary shell commands, regardless of whether the expression is wrapped in quotes.

**Incorrect:**

```yaml
- name: Print title
  run: echo "Title: ${{ github.event.pull_request.title }}"
```

**Correct:**

```yaml
- name: Print title
  run: echo "Title: ${PR_TITLE}"
  env:
    PR_TITLE: ${{ github.event.pull_request.title }}
```

When the value is passed through an environment variable, it is treated as data rather than code, preventing injection.

For `actions/github-script` steps, pass values through the `env` block and access them via `process.env` instead of using template expressions in the script body:

**Incorrect:**

```yaml
- uses: actions/github-script@...
  with:
    script: |
      const title = "${{ github.event.pull_request.title }}";
```

**Correct:**

```yaml
- uses: actions/github-script@...
  env:
    PR_TITLE: ${{ github.event.pull_request.title }}
  with:
    script: |
      const title = process.env.PR_TITLE;
```

### Dangerous triggers

The `pull_request_target` and `workflow_run` triggers run in the context of the base repository and have access to repository secrets. If a workflow triggered by `pull_request_target` checks out the pull request's head ref and runs code from it, an attacker can execute arbitrary code with access to secrets.

- Avoid `pull_request_target` or `workflow_run` unless your workflow genuinely needs access to repository secrets to operate on a pull request (for example, to comment on the PR or manage labels).
- Never check out the pull request's head ref (`github.event.pull_request.head.ref`) in a `pull_request_target` workflow and then run code from that checkout.
- If `pull_request_target` or `workflow_run` is necessary, document the justification inline with a comment explaining why the trigger is safe in context.

### Excessive permissions

Workflow and job permissions should follow the principle of least privilege. Every workflow file should include a top-level `permissions: {}` block that grants no permissions, with individual jobs declaring only the specific permissions they need. Omitting a `permissions` declaration entirely is not sufficient.

**Incorrect:**

```yaml
permissions:
  contents: write

jobs:
  lint:
    # This job only reads code, it doesn't need write access.
    runs-on: ubuntu-latest
```

**Correct:**

```yaml
permissions: {}

jobs:
  lint:
    runs-on: ubuntu-latest
    permissions:
      contents: read
```

### Artipacked credentials

The `actions/checkout` action persists credentials by default so that subsequent git operations can authenticate. If the checkout directory is later uploaded as an artifact (or its contents are otherwise exposed), the persisted credentials can be leaked.

Always set `persist-credentials: false` on `actions/checkout` unless subsequent steps in the same job genuinely need to perform authenticated git operations (such as pushing commits).

**Correct:**

```yaml
- uses: actions/checkout@...
  with:
    persist-credentials: false
```

If the job needs persistent credentials (for example, to push built files), set `persist-credentials: true` explicitly so the intent is clear and auditable, and include an accompanying comment.

### GitHub environment manipulation

Writing to `$GITHUB_ENV` or `$GITHUB_PATH` from a shell script is dangerous if the input is user-controlled, because an attacker can inject arbitrary environment variables or prepend to `PATH`.

- Only write to `$GITHUB_ENV` or `$GITHUB_OUTPUT` with values that are fully controlled by the workflow, not with values derived from pull request content, issue bodies, commit messages, or other user-controllable inputs.
- If you must process user-controllable input, validate and sanitize it before writing to these files.

### Unpinned uses

All third-party actions must be pinned to a full commit SHA, not a tag, branch, or floating version reference such as `v6`. Tags can be moved or deleted as well as added, meaning a tagged reference could silently point to different (potentially malicious) code in the future.

**Incorrect:**

```yaml
- uses: actions/checkout@v6
```

**Correct:**

```yaml
- uses: actions/checkout@de0fac2e4500dabe0009e67214ff5f5447ce83dd # v6.0.2
```

Always include a version comment after the SHA to make the pinned version human-readable. When updating an action, update both the SHA and the version comment.

### Cache poisoning

Using GitHub Actions caching in workflows that produce release artifacts is risky. A cache can be poisoned by an attacker in a separate workflow, allowing the poisoned cache to inject malicious content into a release.

Avoid using `actions/cache` or built-in caching features in workflows that build and publish packages or release artifacts. If caching is necessary in such workflows, ensure the cache key is scoped tightly and the cache contents are verified before use.
