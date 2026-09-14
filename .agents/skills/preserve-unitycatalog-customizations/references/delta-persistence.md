# 14454066 — Delta batch deletion

`server/src/main/java/io/unitycatalog/server/persist/DeltaCommitRepository.java` changes `deleteCommitsUpTo` and `deleteCommits`: direct `DELETE ... LIMIT` becomes deletion by IDs selected through a nested subquery, ordered by `commit_version ASC` and limited by `:numCommitsPerBatch`, using alias `delete_batch`.

Preserve bounded deletion, table isolation and the version bound for `deleteCommitsUpTo`. Test more than one batch, the version boundary and empty results against the actual database. Do not infer universal SQL portability from this patch.


For UUID column mapping changes from `de2e995a`, also read [MinIO and UUID mapping](minio-s3.md).
