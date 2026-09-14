# de2e995a — MinIO/static credentials and UUID mapping

Java paths below are relative to `server/src/main/java/io/unitycatalog/server/`.

- `service/credential/aws/AwsCredentialVendor.java`: selects `StaticAwsCredentialGenerator` when access key and secret key are both nonempty, replacing the previous session-token condition. Static MinIO credentials should work without STS or a mandatory session token. Inspect selection precedence when adapting; legitimate AWS role/STS flows must continue working.
- `utils/ServerProperties.java`: removes the session-token requirement from validation of static keys. This checks presence, whereas the vendor checks nonempty keys.
- `service/credential/CloudCredentialVendor.java`: returns an empty string for a null session token. The patch comment cites Spark connector 0.4.0 building Hadoop properties and expecting a non-null token. Preserve client compatibility or verify that the current implementation provides an equivalent fix.
- `helm/templates/server/_config.tpl`: conditionally emits `s3.endpoint.<index>` and `s3.sessionToken.<index>` when configured. Keep the endpoint available for S3-compatible storage and both settings optional.
- `persist/dao/DeltaCommitDAO.java`: removes hardcoded `BINARY(16)` definitions from `id` and `table_id`, allowing dialect-based UUID mapping. Existing database schema compatibility still requires verification; this patch does not supply a migration.

When changing these paths, validate static keys without token/role/STS, static keys with a token, role-based AWS credentials, returned token handling, Helm rendering with optional settings present/absent, and UUID persistence on the deployment database.

