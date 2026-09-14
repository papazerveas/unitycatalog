# f9ccaa1e — Helm/Gitea notes

`.gitignore` adds `unitycatalog-*.tgz`. `AGENT.md` adds Gitea Helm upload and repository add/update/search examples. The earlier notes include Harbor OCI Helm publishing and Docker buildx commands. These are documentation changes, not evidence of a successful deployment or a chart-version change.

Read `.agents/skills/AGENT.md` if present, or `git show f9ccaa1e:AGENT.md`, for the original operational examples. Image tags and chart versions there are historical: verify current configuration before reusing them. Do not execute publishing commands as part of a history review.


For optional endpoint/session-token rendering in Helm templates, also read [MinIO configuration](minio-s3.md).
