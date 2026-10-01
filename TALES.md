# Tales source project (format v1)

Pages and components have stable IDs inside their JSON files. Filenames are locators.
Update tales.json when moving files or adding/deleting pages and components. Preserve
orderKey values unless changing order. Keep shared component definitions in components,
and instance references/overrides in pages. Do not add revision counters or CRDT blobs.

The schemas directory describes envelopes and hierarchy. The Tales CLI also validates
component definitions, Recipes, slots, tokens, cycles, SVG and cross-file references.
From a Tales checkout: bun apps/local-backend/src/cli.ts validate /path/to/this/project

Manual Save and optional Auto Save use three-way merge. If both the editor and files
change the same field, Save writes Git diff3 markers (editor/base/disk). Resolve every
marker, then Save again to validate and apply the result. Keep arrays such as variants
and slots in their meaningful order; conflicting array edits require a decision.

Wait for Files saved before staging/committing. Run git diff --check and the validator.
Tales never stages, commits, checks out branches or pushes. Normal Git merge/rebase
conflicts are resolved in these files, then reconciled through the next Save.
The .tales directory is private recovery state and must remain ignored.

An arbitrary editor can race a multi-file save. Finish an agent batch before pressing
Save; pause Auto Save during that batch, or use separate Git worktrees. Hash checks
detect many races, but are not an OS-level compare-and-swap for unrelated writers.
