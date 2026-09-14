---
name: check-unitycatalog-updates
description: Check whether this Unity Catalog checkout has upstream commits, newer tags, or newer releases; use for update checks and upgrade planning, not for applying updates.
---

# Check Unity Catalog updates

Determine whether the current checkout is behind the relevant Unity Catalog upstream.
This skill is read-only: do not fetch, pull, merge, rebase, change remotes, or edit repository files.

## Workflow

1. Inspect the current branch, commit, configured remotes, and repository status.
2. Prefer a configured remote named `upstream`; otherwise use `origin`. If `origin` is a fork, compare against the canonical Unity Catalog repository `https://github.com/unitycatalog/unitycatalog.git` without modifying remotes.
3. Use read-only remote queries to compare the current commit with the remote default branch and list newer tags. Do not assume that a newer tag is automatically compatible.
4. Check the canonical GitHub releases page/API when release information is needed. Distinguish published releases from tags and prereleases.
5. Report:
   - current branch and commit;
   - remote/default branch status, including ahead/behind counts when available;
   - newest relevant stable tag and any newer prerelease;
   - whether an update exists;
   - concise next steps, including compatibility checks before upgrading.

## Useful commands

```sh
git branch --show-current
git rev-parse HEAD
git status --short
git remote -v
git ls-remote --symref <remote> HEAD
git ls-remote --heads <remote>
git ls-remote --tags --refs <remote>
```

For GitHub release metadata, use the repository's releases page or API and cite the URL in the report when links are available. Do not treat local Helm package versions as Unity Catalog application versions unless the user specifically asks for Helm chart updates.

## Safety and interpretation

- Never expose credentials or private remote URLs in the report.
- A remote query can fail because of network access or authentication; report that limitation clearly rather than inferring that no update exists.
- If the checkout contains uncommitted changes, mention them and avoid recommending an update command that could overwrite them.
