# Contributing

Thank you for your interest in contributing to POTOS! Your efforts to improve the project are highly valued and appreciated. This guide will help you navigate the contribution process effectively.

## If You Want To

**Fix Bugs**

Found a bug? Help us fix it by submitting a pull request (PR). Follow the steps outlined in the **Operation Guide** section below.

## Operation Guide

### Submitting Issues

1. Go to the [GitHub Issues page](https://github.com/FISCO-BCOS/FISCO-BCOS/issues).
2. Provide a clear and concise description of the issue.
3. Include steps to reproduce the problem, if applicable.

### Submitting Pull Requests (PRs)

1. Fork the repository.
2. Create a branch for your changes (see [branching strategy](#Branching-Strategy) below).
3. Make your changes and test them thoroughly.
4. Submit a PR to the appropriate branch (see [branching strategy](#Branching-Strategy) below).
5. Wait for the community to review your PR.

### Branching Strategy

Our branching strategy is based on [git-flow](https://jeffkreeftmeijer.com/git-flow/):

- **master**: Latest stable branch.
- **dev**: Stable branch awaiting release (merged into `master` upon release).
- **release-***: Branch for preparing a new release.
- **feature-***: Branch for developing a new feature.
- **bugfix-***: Branch for fixing a specific bug.

**Scenario: Fixing Bugs**

1. **Fork** the repository.
2. **Create** a branch named `bugfix-xxxx` from your fork's `master` branch.
3. **Fix** the bug.
4. **Test** your changes.
5. Submit a **PR** to the `dev` branch.
6. Wait for the community to review and merge your PR.

**Scenario: Developing a New Feature**

1. **Fork** the repository.
2. **Create** a branch named `feature-xxxx` from your fork's `dev` branch.
3. **Develop** the feature.
4. Regularly **pull** updates from the `dev` branch into your feature branch.
5. **Test** your changes.
6. Submit a **PR** to the `dev` branch.
7. Wait for the community to review and merge your PR.

## Development Guide

**Coding Guidelines**

To ensure consistency and maintainability, please follow these guidelines:

- Adhere to the coding standards defined in the [.clang-format](https://github.com/FISCO-BCOS/FISCO-BCOS/blob/master/.clang-format) file.
- Write well-documented and thoroughly tested code.
- Use descriptive commit messages in the format:
  - `<type>(scope): <subject>`
  - Example: `feat(protocol): add new consensus mechanism.`
  - Types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `chore`.
  - Scope: Module or package name.
  - Subject: Short description of the change.

**Code Formatting**

- Use the [Clang-Format](https://clang.llvm.org/docs/ClangFormat.html) tool (version 17.0 or higher recommended) to format your code.
- Refer to the [.clang-format](https://github.com/FISCO-BCOS/FISCO-BCOS/blob/master/.clang-format) file for formatting rules.
