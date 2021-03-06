# labels

Python 3.6 CLI app to manage GitHub issue labels 📝

## Installation

**labels** is available for download from [PyPI][PyPI] via [pip][pip]:

```text
$ pip install labels
```

## Authentication

The labels CLI connects to the GitHub API to modify issue labels for a GitHub
repository. Please [create your own personal API token][create token] and
choose the correct token scope based on whether you want to manage issue
labels for a public or a private repository. Then set up two environment
variables in your terminal:

```bash
$ export LABELS_USERNAME="<GITHUB_USERNAME>"
$ export LABELS_TOKEN="<GITHUB_TOKEN>"
```

## Usage

Once you set up the environment variables, you're all set and ready to use
the labels CLI to manage issue labels for a GitHub repository. The CLI comes
with two commands: ``fetch`` and ``sync``.

Both require you to specify the owner and the name of the GitHub repository
using CLI options:

```text
-o, --owner TEXT     GitHub owner name
-r, --repo TEXT      GitHub repository name
```

### Fetch

When you're using **labels** for the first time, you want to fetch
information about the existing labels for your GitHub project. The CLI will
then write a [TOML][toml] file to your computer with the retrieved
information. The default filename for this file is ``labels.toml`` in your
current working directory and can be changed by passing the
``-f, --filename PATH`` option followed by a path.

```text
$ labels fetch -o hackebrot -r pytest-emoji
```

```toml
[bug]
color = "ea707a"
description = "Bugs and problems with pytest-emoji"
name = "bug"

["code quality"]
color = "fcc4db"
description = "Tasks related to linting, coding style, type checks"
name = "code quality"

[dependencies]
color = "43a2b7"
description = "Tasks related to managing dependencies"
name = "dependencies"

[docs]
color = "2abf88"
description = "Tasks to write and update documentation"
name = "docs"

["good first issue"]
color = "bfdadc"
description = "Tasks to pick up by newcomers to the project"
name = "good first issue"
```

### Sync

Now that you have a file on your computer that represents your GitHub issue
labels, you can edit this file and then run **labels sync** to update the
remote repository. But first let's look into how that works... 🔍

Representation of a GitHub issue label in the written TOML file:

```toml
[docs]
color = "2abf88"
description = "Tasks to write and update documentation"
name = "docs"
```

The section name represents the name of the label for that repository and is
identical to the ``name`` field when running ``labels fetch``. The fields
``color``, ``description`` and ``name`` are parameters that can be modified
with the
**labels** CLI.

- ``name`` - The name of the label
- ``description`` - A short description of the label
- ``color`` - The hexadecimal color code for the label, without the leading ``#``

You can make the following changes to issue labels for your repo:

- You can **delete** a label by removing the corresponding section from the
labels file 🗑
- You can **edit** a label by changing the value for one or more parameters for
that label 🎨
- You can **create** a new label by adding a new section with your desired
parameters 📝

It is recommended to check first what **labels** would do, before making any
changes, with the ``dryrun`` CLI option:

```text
-n, --dryrun         Do not modify remote labels
```

Example usage:

```text
$ labels sync -n -o hackebrot -r pytest-emoji
```

```text
This would delete the following labels:
  - dependencies
This would update the following labels:
  - bug
  - good first issue
This would create the following labels:
  - duplicate
This would NOT modify the following labels:
  - code quality
  - docs
```

Once you're happy with your changes, run the ``sync`` command again without
the ``-n`` option. After **labels** synced the labels with your GitHub
repository, it will write the updated information to your labels file, so
that section names match the ``name`` parameter etc.

If **labels** encounters any errors while sending requests to the GitHub API,
it will print information about the failure and continue with the next
label until it processed all of the labels.

## Community

Are you interested in contributing to the **labels** CLI app, or helping us
improve our documentation, or have ideas for how to improve the project?

Read our [contributing guide][contributing] and check out the
[good first issue][first] label for tasks, that are good candidates for your
first contribution to **labels**. Your contributions are greatly
appreciated! Every little bit helps, and credit will always be given!

Please note that **labels** is released with a
[Contributor Code of Conduct][code of conduct]. By participating in this
project you agree to abide by its terms.

## License

Distributed under the terms of the MIT license, **labels** is free and open
source software.

[code of conduct]: https://github.com/hackebrot/labels/blob/master/.github/CODE_OF_CONDUCT.md
[contributing]: https://github.com/hackebrot/labels/blob/master/.github/CONTRIBUTING.md
[create token]: https://blog.github.com/2013-05-16-personal-api-tokens/
[first]: https://github.com/hackebrot/labels/labels/good%20first%20issue
[toml]: https://github.com/toml-lang/toml
[PyPI]: https://pypi.org/
[pip]: https://pypi.org/project/pip/
