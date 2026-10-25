# Release Notes

## v1.0.7
GH#1100 migration wave — retire `default_from_previous` on the `source` slot.

The slot is now declared **required**. The body already read `{{step.context.source}}`; what changed is that the value must be bound explicitly rather than falling back to whatever step happened to run before. `default_from_previous` was positional resolution wearing a declaration (K-045 Option A), and K-073 revoked it.

**This prompt stays in the C7 source-slot registry, and its `since` stays 1.0.5.** That version records when the bindable slot was introduced, which is not changed by removing the fallback. Four consumers invoke this prompt; three do not bind the slot and will need to when they bump.

## v1.0.6
GH#845 — republish with American English (en-US) content, completing the source-only GH#805 flip that never reached the Hub. Copy only — no functional or behaviour change.

## v1.0.5
GH#781 / K-045 — declare a `source` input slot (`context_params.source: { default_from_previous: true }`) and read `{{step.context.source}}` instead of positional `{{steps.previous.output}}`, so a consuming workflow can map the content to headline explicitly via `from_step`. Backward-compatible: unbound `source` defaults to the previous step output, so linear consumers are unchanged.

## v1.0.4
GH#657 Framing B — republish wave. Bundle now ships `dependencies: []` in its signed manifest (injected by `publish-skrpt.mjs` for `--shared` publishes), so the App's dep-referenced install pipeline (post-PR #47) accepts it on standalone update via Hub Update-all. No content changes.

## v1.0.3
Fix-forward: the v1.0.2 publish stored either no content or JSON-invalid content in D1's object_manifest column for this dep. Root cause was `d1Execute` shelling out via `wrangler d1 execute --command="..."` — bash double-quote parsing silently mangled JSON-encoded markdown containing backslashes or multi-byte UTF-8. Fixed by routing SQL through `--file=` so wrangler reads bytes directly. v1.0.3 republishes against the fixed pipeline. No content changes in the signed bundle; only the D1 metadata projection.

## v1.0.2
GH#651 Hub publish — republish to populate `nodes[].content` in object_manifest for prompt nodes. The /api/shared/<slug>/<v>/metadata endpoint now exposes `nodes[].content` for type='prompt' (GH#650 / GH#651), used by the engine validator's dep-aware loop-body `{{loop.item}}` interpolation check. Pre-fix publish-skrpt.mjs stripped content uniformly; aba4ccd7 preserves it for prompts. No content changes from v1.0.1; signed-bundle data identical, only D1 object_manifest gains the `content` field.

## v1.0.1
GH#645 Row 3a — republish with `manifest.id` aligned to Hub catalogue UUID. Pre-K-037 v1.0.0 bundle carried a local `manifest.id` that drifted from the catalogue UUID. Row 5 (`0d9c9dbe`) reconciled the source file but not the signed bundle; v1.0.1 ships the corrected `manifest.id` so the batch-of-81 consumer migrations (GH#645 Row 3b) can pin this version and pass engine STEP 4d. No content changes.

## v1.0.0
Initial catalogue release as independent shared object (GH#612 Wave 3).
