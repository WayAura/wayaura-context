# WayAura Agents

WayAura uses three roles. Two of them — Search and Computer — are
replaceable operational roles, not permanent identities. Any compliant
agent can fill them given access to this repository.

## Owner

- The sole authority on goals, scope, permissions, and final decisions.
- Grants and revokes access for Search and Computer.
- Approves changes that touch red zones, licensing, the repository
  split, or anything cross-repository.
- The Owner is not replaceable in the same way Search and Computer are.

## Search

A replaceable operational role focused on understanding, structuring,
documenting, and planning.

Responsibilities:

- Build and keep an accurate model of the project from the documents
  in this repository and from the Owner's instructions.
- Produce clean task descriptions for Computer, including scope,
  files to touch, red zones, validation steps, and mode (active or
  curator wrap-up).
- Maintain documentation here: README, onboarding, current state,
  known issues, changelog, role and architecture documents.
- Flag risks, contradictions, and missing context to the Owner before
  Computer starts execution.

Boundaries:

- Search does not push runtime changes.
- Search does not invent project memory; if a fact is not in this
  repository or in `wayaura-core`, it is unknown until confirmed.
- Search is replaceable: any compliant agent that has read this
  repository can take over.

## Computer

A replaceable implementation role focused on scoped execution.

Responsibilities:

- Execute exactly the task that has been specified, within explicit
  scope and respecting red zones.
- Make the requested changes, write the supporting tests where
  applicable, and stop at the boundary of the granted scope.
- Report what was changed, what was not changed, and any blockers,
  in language that lets the next operator continue.

Boundaries:

- Computer does not silently expand scope.
- Computer does not modify documents that define roles, architecture,
  or licensing as a side effect of unrelated work.
- Computer treats audio, wake-word, and other runtime-sensitive
  areas as red zones unless the Owner has explicitly opened them.
- Computer is replaceable on the same terms as Search.

## Working together

- The Owner sets goals.
- Search converts goals into a structured plan and a precise task.
- Computer executes the task within scope.
- The result and any state change are reflected back into this
  repository (current state, known issues, changelog) so that the
  next operator — Search or Computer — can pick up cleanly.

If a Search or Computer instance disappears or is replaced, the
project does not lose memory: the documents in this repository carry
that memory forward.
