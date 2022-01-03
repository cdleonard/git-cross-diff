git cross-diff - Diff logs between two branches

Usage:
------
::

    git cross-diff [options] [--] [path...]
    git cross-diff [options] OLD_HEAD
    git cross-diff [options] OLD_HEAD NEW_HEAD

This script will diff two commit logs. It is sort of equivalent to the following::

    diff -u <(git log OLD_BASE..OLD_HEAD) <(git log NEW_BASE..NEW_HEAD)

Running the above command generates non-interesting diffs. For example
identical patches rebased top of slightly different trees will have different
commit hashes, but this is not interesting. This script will filter such noise
and provide an answer to "what was changed by this rebase".

In newer versions of git similar functionality is provided by
`git range-diff <https://www.git-scm.com/docs/git-range-diff>`_

Options:
--------

-h, --help                      Show this message.

--old, --old-head OLD_HEAD      Specify the OLD_HEAD commit.
--new, --new-head NEW_HEAD      Specify the NEW_HEAD commit.
--old-base OLD_BASE             Specify the OLD_BASE commit.
--new-base NEW_BASE             Specify the NEW_BASE commit.
--base BASE                     Set both OLD_BASE and NEW_BASE to same commit.

--path PATH                     Add this to the paths list.

-p, --patch, --no-patch         Also diff the patches (default false).
--diff-bases, --no-diff-bases   To diff the two BASEs or not (default if --patch).
--diff-heads, --no-diff-heads   To diff the two HEADs or not (default if --patch).
--include-stable-patch-id       Include stable patch-id for each commit (off by default).
--exclude-stable-patch-id       Explicitly disable previous option.
--include-commit-hash           Include commit hash for each commit (off by default).
--exclude-commit-hash           Explicitly disable previous option.

The OLD_HEAD and NEW_HEAD can also be specified as non-option arguments.

Paths are specified after a double dash (--) or with --path. This can be used
to restrict examination to a set of paths.

The following rules are used to deal with incomplete arguments:

* The OLD_HEAD is mandatory.
* If NEW_HEAD is not specified it defaults to the current git HEAD.
* If OLD_BASE is not specified it defaults to `git merge-base OLD_HEAD NEW_BASE`.
* If NEW_BASE is not specified it defaults to `git merge-base NEW_HEAD OLD_BASE`.
* If neither OLD_BASE nor NEW_BASE are specified they both default to `git merge-base OLD_HEAD NEW_HEAD`.
