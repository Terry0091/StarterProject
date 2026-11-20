# Releases

Every image this team has released, identified by **digest**. Session 9 deploys
to Staging by the digest in the last row; Session 10 promotes the **same**
digest to Production. A tag is a label — the digest is the artifact.

| Version | Digest (`sha256:…`) | Commit | Date | CI run | Attestation verified by |
|---|---|---|---|---|---|
| 0.2.0 | `sha256:` | `` | | [run]() | `scripts/verify-release.sh` — name |

How to fill a row: open the `publish` job of the main-branch run that created the
release, copy the **Digest** from the run summary, then run

```bash
bash scripts/verify-release.sh ghcr.io/<owner>/<repo>:<version> <owner>/<repo>
```

and write your name in the last column only if it printed `PASS`.

## Retention policy

| What | Kept | Why |
|---|---|---|
| Images with a SemVer tag (`0.2.0`) | **forever** | Rollback target and audit trail: we must be able to say what ran in Production on any date |
| Images tagged only `sha-<commit>` | until the end of the semester, then the oldest are deleted **by hand, reviewed by a second person** | Enough to bisect recent main-branch builds; they were never released |
| Untagged entries in the package | **not deleted automatically** | The provenance and SBOM attestations are stored next to the image as untagged entries. A cleanup job that deletes "untagged versions" deletes the evidence that the release was built by this pipeline |
| Release-candidate artifact in Actions | 1 day | A hand-over between two jobs, not an archive |
| SBOM artifact in Actions | 90 days | Also attached to every released image as a signed attestation |

> **Why there is no automatic cleanup job.** `actions/delete-package-versions`
> identifies container versions by digest, not by tag, so a regex cannot protect
> `0.2.0`; and its "untagged only" mode would remove attestations. Automating a
> deletion you cannot express safely is how a team loses its audit trail. Delete
> by hand, from the package page, with a reviewer — and record it here.

> Agreed by: ______________________   Date: __________   Review: next semester
