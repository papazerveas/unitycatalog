# Agent Instructions

## Default behavior

- Do not modify, create, delete, format, or rename any file unless the user explicitly asks for a file change.
- If the user asks only for analysis, explanation, review, or investigation, inspect the repository and report findings without changing files.
- Before making any requested edit, briefly state which files you intend to change and why.
- Keep changes narrowly scoped to the user's request. Do not include opportunistic refactors, formatting churn, dependency updates, or unrelated cleanup.
- Preserve existing user changes. Never revert work you did not make unless the user explicitly asks for that revert.

## Repository context

- The active branch `fix/uc-minio-managed-external-credentials` differs from `main` in a small set of server and Helm files.
- The branch focuses on S3-compatible storage, especially MinIO/static AWS credentials:
  - Helm server config can emit optional `s3.endpoint.<index>` and `s3.sessionToken.<index>` values.
  - Static S3 access key and secret key credentials should work without requiring a session token.
  - Static S3 credentials should not require STS or an AWS role ARN.
  - When returning AWS credentials to clients, avoid returning a null `sessionToken`; use an empty string when no token exists.
- The branch also adjusts delta commit persistence:
  - Batched deletes use a subquery by `id` ordered by `commit_version` instead of relying on `DELETE ... LIMIT`.
  - UUID column definitions in `DeltaCommitDAO` are left to the database dialect instead of forcing `BINARY(16)`.

## Caution areas

- Be careful with credential handling. Do not log or expose access keys, secret keys, session tokens, role ARNs, or endpoint-specific secrets.
- When changing S3 credential behavior, consider both AWS STS role-based credentials and S3-compatible static credentials.
- When touching SQL or DAO mappings, consider database portability across the repository's supported test/runtime databases.
- When editing Helm templates, preserve optional configuration behavior and avoid requiring values that are intentionally optional.

# build image

## change dockerfile

```dockerfile
# Keep Coursier dependencies under HOME so the generated runtime classpath
# points to files that exist in the non-root runtime image.
ENV COURSIER_CACHE=$HOME/.cache/coursier

# before 
WORKDIR $HOME
```

## docker

```sh
docker buildx build \
  --builder test-buildkit --platform linux/amd64 \
  -t harbor.retailzoom.local/library/unitycatalog:main-minio-fix-20260522 \
  --push   .
```

# push helm to harbor

```bash
helm registry login harbor.retailzoom.local
helm dependency update ./helm
helm package ./helm
helm push unitycatalog-0.0.1-pre.1.tgz oci://harbor.retailzoom.local/helm
```