# Commit provenance and upstream merge

Three commits have author name Sotiris Papazerveas: `f9ccaa1e`, `dd86c798`, `a8324ac0`. Two related earlier commits have author `root` with the same email (`sotiris.papazerveas@retailzoom.net`): `de2e995a` and `14454066`. Include these when tracing the custom fixes.

## a8324ac0 — Upstream merge

Parents: `14454066` (feature) and `cd6fec68` (then-main). The first-parent diff includes a large upstream import (Delta APIs, connectors, build, tests and documentation); do not attribute all that work to Sotiris. The second-parent diff retains the six custom files documented in minio-s3.md and delta-persistence.md.

```sh
git diff a8324ac0^1 a8324ac0 --stat
git diff a8324ac0^2 a8324ac0
```

Do not automatically cherry-pick this merge. Compare both parents when assessing custom changes.

