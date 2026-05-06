# Known Issues

## Active issues

- Repository split has been defined conceptually and must now be enforced cleanly in GitHub.
- Some historical materials came from a flat staging area and may still contain pre-split wording.
- Runtime-sensitive files exist, but red-zone handling must remain conservative during repo normalization.
- Documentation drift remains a risk if context and core repos are edited independently without consistency review.

## Ongoing caution points

- Do not treat old session memory as reliable unless reflected in current docs.
- Do not move runtime files into the context repo.
- Do not duplicate the full continuity layer into the core repo.
