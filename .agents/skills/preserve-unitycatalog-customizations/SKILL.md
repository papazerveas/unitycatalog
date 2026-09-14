---
name: preserve-unitycatalog-customizations
description: Preserve Sotiris Papazerveas's Unity Catalog MinIO, Delta persistence, Docker and Helm changes during upstream synchronization, conflict resolution and maintenance of the affected code.
---

# Preserve Unity Catalog customizations

Historical baseline: branch `fix/uc-minio-managed-external-credentials` at `f9ccaa1e`. Inspect current code before assuming these behaviors still exist. Preserve their intent when adapting to upstream; do not overwrite newer files with historical copies.

## References

Read only the references relevant to the task. For a complete upstream sync review, read all five.

- [Commit provenance and merge](references/history.md): author attribution, branch history and interpreting the merge parents.
- [MinIO/S3 and UUID mapping](references/minio-s3.md): static credentials, token handling, optional Helm settings and database UUID mapping.
- [Delta persistence](references/delta-persistence.md): batch deletion, version bounds and database validation.
- [Docker](references/docker.md): Coursier cache paths and non-root runtime startup.
- [Helm publishing](references/helm-publishing.md): package ignore rule, operational notes and historical deployment examples.

## Using and maintaining this knowledge

1. Inspect current branch, index and working tree. Staged, unstaged and untracked changes are separate from this history. Do not assume all local changes should be committed together.
2. Read relevant patches with `git show <commit> -- <path>` and compare them with the proposed upstream version. An equivalent upstream implementation may replace a custom patch; preserve behavior rather than exact lines.
3. Report each affected behavior as preserved, replaced by an equivalent implementation, missing or unverified, with file evidence. Commit ancestry alone does not prove subsequent edits preserved behavior.
4. For an authorized implementation, run relevant checks in the selected references and state which actually ran. This skill does not itself authorize merge, commit, push or deployment.
5. When extending this knowledge, inspect actual diffs and record their hashes and author provenance. Never store credential values. Avoid treating historical branch positions, deployment destinations or versions as current requirements.
