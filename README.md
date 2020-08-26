# Contributing to Open Source projects

The following are guidelines for contributing to Open Source projects maintained by TTCO Holding Company, Inc.

## Copyright Attribution

Copyright for these projects is held by "TTCO Holding Company, Inc. and Contributors", and copyright notices should reflect this.

## Licensing

Each publicly visible repository will declare its license.
- The [Apache License, Version 2.0](https://opensource.org/licenses/Apache-2.0) is the preferred license for source code.
- The [Creative Commons Attribution 4.0 International Public License (CC-BY)](https://creativecommons.org/licenses/by/4.0/) is preferred for documentation.

## File Headers

Each file that contains source code should contain the following header using the appropriate comment style for its respective language.

````
// SPDX-License-Identifier: Apache-2.0
// Copyright [year project was first published], TTCO Holding Company, Inc. and Contributors
// Licensed under the Apache License, Version 2.0. See LICENSE.TXT in the repository root for license information.
````

### Exceptions to File Headers
Some files may be copied from other projects when a binary reference is either unavailable or impractical.

Please ensure the following is true of any files copied into open source projects:
- The license of the file is compatible with the license of the project.
- The license of the file is intact.
- The appropriate attributions and third-party notices are included in the repository.

## Code Style

Each repository that accepts contributions will link to the appropriate coding style(s) for its contents.

## Repository Settings

Each publicly-visible repository should have the following settings:
- Default branch named `main`.
- A Branch Protection Rule applied to `main` that enforces the following:
    - `Require pull request reviews before merging` with at least 1 reviewer
    - `Dismiss stale pull request approvals when new commits are pushed`
    - `Require signed commits`
    - `Require linear history`