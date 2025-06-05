For all projects it is required to have repository under [research-centre-rez](https://github.com/research-centre-rez) organization. 

Name of the repository should be as short as possible but relevant to its content. Inspiration can be taken from [existing repositories](https://github.com/orgs/research-centre-rez/repositories).

TODO: unify naming convention between repos ... e.g.: underscore vs. hyphen, uppercase/lowercase usage, snake_case vs. CamelCase, prefixes, suffixes...

# Branching
- feature branches
- bugfix branches

# Pull requests

- PR should be rather small so that superviser can review it easily.
- PR should be created as soon as new branch, with WIP: status, withoout reviewer so it's clear what is being worked on.
- After code review, there should be a stop for features. Any new code goes to a different branch stemming from the one being in review (so the developer is not blocked by the reviewer).

Merging into master/main branch should go thru PR done by your supervisor
Before you create PR run `pytest`, linter. Clean your debug messages.

## Code Review and Pull Request Process
To ensure an efficient workflow, please follow these guidelines when working with Pull Requests (PRs):

### Approved PRs
If your PR is marked as "approved", it means that I do not wish to review it again. In most cases, these are only minor comments that do not require further discussion. The process should be as follows:

Review all comments:
- If you agree with a comment and implement the suggested change, click "Resolve".
- If you disagree, reply directly to the comment explaining why the original solution is preferable.
- After addressing all comments, merge the PR and delete the branch.

All relevant information and discussions will remain available on GitHub for future reference.

### Non-Approved PRs
If the PR is not approved, it means there are some points that must be revised. In such cases:
- The requested changes are considered necessary and must be either implemented (in which case I will review the updated code), or
- We should discuss the matter and agree on a suitable compromise.

This process is designed to streamline collaboration while ensuring code quality. Please follow it consistently.

# Tags
Tags should be used for every realeased version and equipped with describing commit message.
Tag name could have prefix (e.g. stable, dev, ...) and must have [version number](https://semver.org)
