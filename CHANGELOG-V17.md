# Changelog

All notable changes to Translation Manager for Umbraco v17, by release tag.

## 17.9.1 (`v17.9.1`)

A performance fix for large multi-site, multi-culture installs.

- Fix: browsing the content tree in the backoffice could make the site progressively
  slower over the course of a day — sometimes badly enough to freeze the backoffice and
  lose editor work — on a site with a translation set spanning several sites and
  cultures. Each node loaded in the tree triggered a lookup whose response kept growing,
  because a duplicate site entry was being added to that set's cached data every time and
  never cleared. Reported to us directly by a customer running a large multi-language
  site; fixed by comparing against the right culture when checking whether an entry
  already existed, and by making sure that kind of per-request lookup can no longer write
  back into the shared cache at all, regardless of what changes there in future

## 17.9.0 (`v17.9.0`)

A glossary for consistent terminology, granular Translation Memory permissions, and a
round of translation-correctness fixes.

- Add: Glossary — a global list of terminology, managed from its own item in the
  Translations section. Add terms with a value (or a "do not translate" flag) per
  installed language, and import/export as CSV (one language pair at a time — a
  worksheet a translator can fill in) or TBX across every language at once, for
  termbases exported from another tool. The glossary section only appears once a
  connector that reads it is configured — right now that's the AI connector; DeepL,
  Google and Microsoft glossary support is still on the way
- Add: two new granular permissions, "View glossary" and "View translation memory" — a
  user needs both translation-section access and the specific permission to see either.
  Administrators get both automatically on upgrade; any other group that wants them
  needs to be granted explicitly. As with any permission change, it only reaches an
  already-signed-in session at their next sign-in
- Improve: Translation Memory moved from Settings into the Translations section,
  alongside Glossary, with its own on/off toggle and item counts on the dashboard
- Fix: creating a translation could get stuck with an unhelpful internal error if you
  pressed Create before choosing a connector — the button now stays disabled until one's
  selected
- Fix: a batch job that came back only partly translated (some content pre-filled from
  translation memory, the rest still out with the provider) could get stuck showing as
  fully reviewed with no way to check the rest back in
- Fix: content with nothing translatable (every page unpublished, or nothing to
  translate at all) used to produce a hollow, confusing job, or fail outright — it now
  completes cleanly and says "Nothing to translate"
- Fix: a translation set with "Hide version choice" ticked silently defaulted new jobs to
  published-only content; new jobs now default to the latest content regardless
- Fix: job submission could fail with "Failed to submit job" if certain internal data had
  been stored as an empty string rather than blank
- Build: the AI connector (`17.6.0`) adds retry-on-failure for transient errors, a
  redesigned Prompt Builder, Fast/Balanced/Quality presets per translator, and support
  for the new glossary

## 17.8.2 (`v17.8.2`)

A clone-to-language action, and a fix for sites also running uSync.Complete.

- Add: for a language with no translation yet, clone the content you're currently viewing straight into it, skipping the translation job pipeline entirely — the same one-click clone action from v13, now in the v17 backoffice
- Fix: installing Translation Manager alongside uSync.Complete 17.4.0 could produce a `400 - Read unrecognized type discriminator id 'TranslationProcessingOptions'` error on translation requests. Both packages share an underlying dependency, and a version mismatch between them was the cause — now resolved
- Fix: the licence check requests Translation Manager makes now identify themselves properly, matching uSync.Complete's behaviour

## 17.8.1 (`v17.8.1`)

Fixes an install problem some sites hit on 17.8.0, plus a first-install translation set guess and a connector-selection fix.

- Fix: 17.8.0's core package pulled in a set of unused ASP.NET Core dependencies it didn't need, which broke installation on some sites. Our build pipeline now pins an exact .NET SDK version and verifies its full dependency graph on every build, so this class of problem can't recur unnoticed
- Add: on a fresh install with no translation set configured yet, Translation Manager now guesses and selects a starter set automatically
- Fix: the create-job dialog now auto-selects the connector when a translation set has only one available, instead of leaving it unselected

## 17.8.0 (`v17.8.0`)

Bulk multi-node translation jobs from the Content section, and a connector settings tidy-up.

- Add: bulk translation dashboard in the Content section — pick multiple content items with the standard document tree picker, review what's selected (name/path, translation set, include-children), and send them into a single translation job; the job-creation config step now lists the content items being sent when more than one is involved
- Improve: connector settings lookups, including marking the unused `ProviderSettingsController` GET endpoints obsolete for removal in v20
- Build: restore, pack, and push each release package individually instead of one whole-solution build/push, so a connector package that isn't published yet only fails that one package instead of blocking the rest
- Build: update connector dependency versions

## 17.7.2 (`release/v17.7.2`)

Translation memory correctness fixes, job-creation validation, clearer error reporting, and a log-noise fix.

- Fix: approved translation memory could silently revert to pending when shared source text was re-translated by another node, and editing a single value on a node deleted translation memory for every other property on that node; node approval also failed to promote memory for shared text owned by a different node's reference
- Fix: the choose-provider step in job creation didn't validate connector selection before enabling Next, so clicking Next while the connector was still resolving (e.g. a set with no default connector) could save a job with no provider set
- Fix: checking a job manually could show two notifications — a generic one and a warning that lost the actual server-side detail; now a single warning uses the real error title/detail from the API, and the same error is also shown as a persistent box on the job page until a successful check or navigation away
- Fix: demote benign "no translation set applies to this save" cases (invariant doctypes, culture-only saves) from a warning back to debug logging, and stop scanning for dirty cultures on doctypes that don't vary by culture, to stop log flooding
- Build: update connector dependency versions (Microsoft, Jumoo.TranslationManager.AI)

## 17.7.1 (`release/v17.7.1`)

Fix a rich text property crash caused by empty markup round-tripping as `null`.

- Fix: an RTE property with empty markup (e.g. a block-only rich text value) could come back from translation as `{"markup": null, "blocks": ...}` instead of `{"markup": "", "blocks": ...}`, which made the node unopenable in the backoffice with `ArgumentNullException: Value cannot be null. (Parameter 'input')` from `RichTextPropertyValueEditor.ToEditor`

## 17.7.0 (`release/v17.7.0`)

Granular translation permissions, connector flexibility, and a security fix for the API endpoints.

- Add: split "approve" (put translated content back as a save) from "publish" (publish it once back) into two separate grantable permissions, so a third party can be given translation work without being able to publish (issue #101). Existing groups that could already manage translations are given both permissions automatically on upgrade, so no one loses access
- Fix: mutating API endpoints (job archive/remove/reset/bulk actions, node deletion and property edits, set save/copy/delete, memory delete, connector settings) previously had no translation-permission check at all, callable by any authenticated backoffice user; they now require the manage-translations permission
- Add: "Lock connector" option next to a set's default connector. When unticked, editors see the connector dropdown in the create-job dialog pre-selected on the set default, and can pick any other active connector. Ticked (the default for existing sets) keeps the current behaviour of forcing the set's connector
- Build: update connector and dependency package versions (Microsoft, Xliff, Jumoo.TranslationManager.AI, Jumoo.Processing, Jumoo.Json)

## 17.6.0 (`release/v17.6.0`)

Rich text editing for HTML translation values.

- Add: edit HTML (rich text) translation values in a Tiptap editor instead of a raw-markup textarea, with basic formatting (bold, italic, headings, lists) and a source-view button for markup the toolbar doesn't cover — no blocks, images, or media embedding

## 17.5.3 (`release/v17.5.3`)

Fix a duplicate custom-element registration that could appear after upgrading, caused by the backoffice client being loaded twice from browser cache.

- Fix: align the `@jumoo/translate` import-map entry with the versioned backoffice entrypoint URL, so a stale cached copy can no longer load alongside the fresh build and register custom elements twice (e.g. `jumoo-tm-settings-connector-menu has already been used with this registry`)

## 17.5.2 (`release/v17.5.2`)

Batch translation memory fix and a connector dependency bump.

- Fix: batch translation memory sometimes incorrectly marked a section as coming from translation memory when it wasn't
- Build: update Microsoft connector dependency

## 17.5.1 (`release/v17.5.1`)

Fixes to the block editor (block list/grid) translation merge, following on from 17.5.0.

- Fix: source (untranslated) text ending up in blocks instead of the translated value, caused by an inverted loopback/variance check when resolving a block's target value
- Fix: orphaned block layout entries left behind after a block is dropped from the translated content, which showed as a "missing block" in the UI
- Fix: block content merge could return `null` from a redundant double-merge pass

## 17.5.0 (`release/v17.5.0`)

Background processing reliability: migrate to Umbraco's recurring background job framework and fix the scope-corruption errors seen during processing.

- Fix: resolve "No AmbientContext was found" errors during background processing — recurring jobs no longer share Umbraco's ambient scope stack with the host or other hosted services
- Fix: background approval items are now processed as the user who queued them, so permission checks no longer fail when a scheduled pass picks them up
- Change: migrate the background item processor and the submitted-job checker to triggerable `IRecurringBackgroundJob`s (replacing the legacy hosted-service base); immediate processing now signals the job directly, removing the queue/semaphore workaround
- Fix: use the real options key for job-option toggles instead of the label text
- Build: update Umbraco.Cms dependencies to 17.5.0

## 17.4.6 (`release/v17.4.6`)

Stability fixes for the background processing queue and translation auto-trigger.

- Fix: skip background processing when a run is already active, to prevent concurrent scope corruption
- Fix: stop translation save-back from re-triggering auto-translate
- Fix: avoid concurrent scope usage in `GetActiveProviders`
- Fix: remove duplicate custom element registrations causing a `DOMException`
- Remove the template-missing warning (we fall back to default, and debugging is available if needed); stop logging the return of a missing template

## 17.4.4 (`release/v17.4.4`)

Small fixes to set settings persistence.

- Fix: "notify creator" toggle not saving
- Fix: "ignore doc types" not saved from set settings
- Fix: typo meant "copy on create" could not be unpicked on a set

## 17.4.2 (`release/v17.4.2`)

Translation memory management UI, new localisations, and several stability fixes.

- Add translation memory entry management, with an edit modal, approved-only filter, and copy-between-connectors support
- Add Spanish, French, German, Danish and Dutch localisations
- Replace native `confirm()` dialogs with Umbraco confirm modals for bulk/entry deletion
- Pass translation memory results back to connectors and show memory data in the UI
- Fix: licence domain check now splits on comma and semicolon, fixing `BadHost` on subdomains
- Fix: XLIFF HTML decoding now preserves `&amp;`
- Fix: pending view should lock the connector selector once one is chosen
- Update DeepL dependency; tidy loading states on the pending view

## 17.4.0 (`release/v17.4.0`)

Adds batch translation memory support and a build script for releases.

- Add support for batch memory translations, with a new `MachineBatchBase` class for connectors
- Add build script to the project
- Fix: quick translate button showing sets that aren't quick-translate enabled

## 17.3.2 (`release/v17.3.2`)

Database column widening for memory storage.

- Make memory columns `NVARCHAR(MAX)` to remove length limits
- Update the Translation Manager AI package

## 17.3.1 (`release/v17.3.1`)

Machine translation and legacy RTE handling improvements.

- Change machine translation behaviour so content is always treated as HTML unless explicitly told otherwise
- Add legacy RTE values to the block translation flow, and upgrade legacy RTE into the "markup" group on translate
- Move away from `MainApplicationUrl` as it can't be relied on
- Align properties in the node view; wrap/guard debug logging on the machine connector
- Update DeepL dependency

## 17.3.0 (`release/v17.3.0`)

Performance and tooling work: centralized package management and responsiveness improvements for large jobs.

- Move to centralized package management (`Directory.Packages.props`)
- Update core to use the newer v17.3 HeyApi-generated client methods
- Improve responsiveness of big job controller calls; convert private results to lists for a small performance gain
- Improve semaphore trimming and progress bar behaviour
- Add more debug logging to the block mapper and property process
- Tidy connector UI and icons

## 17.2.5 (`release/v17.2.5`)

- Add support for single blocks

## 17.2.4 (`release/v17.2.4`)

Locking fixes to stop concurrent processes tripping each other up.

- Add semaphores for locking processes so they don't trip each other up
- Add locks for writing to the version tables
- Fix: `NuXliffSerializer` didn't clean XML before saving a value
- Fix: invalid licence reporting checked the wrong domain
- Add "Add all" button to the language dropdown when there are many languages

## 17.2.3 (`release/v17.2.3`)

UI polish and audit/debugging improvements.

- Add additional audit events for easier debugging
- Add nicer parent naming and browser titles throughout
- Fix: history returned "none" for jobs
- Fix: ensure paths used in blocks are unique
- Handle property groups nested in tabs, and use the actual tab display names

## 17.2.1 (`release/17.2.1`)

UI release with a simplified creation dialog and quick-translate improvements (also covers the 17.2.0 UI release).

- Simplify the job creation dialog
- Use the simple translate view for the quick translate dialog
- Add `useLegacySubmit` option to toggle the new pending workflow on or off
- Add support for a connector "basic view"; simplify connector text and sizing when a connector is locked/restricted
- Fix: block splitting in machine translators was inconsistent
- Fix: provider settings not updating in the clean view
- Fix: couldn't deselect languages in quick translate
- Bump DeepL version

## 17.1.3 (`release/17.1.3`)

Caching and multi-culture fixes.

- Fix: cache now sets IDs correctly for multiple cultures
- Fix: use the global set context consistently across the board
- Fix: don't re-add site IDs to a site if they're already present

## 17.1.0 (`release/17.1.0`)

Introduces one-click translate and per-request throttling for connectors.

- Add one-click translate setup
- Add per-request throttle support (requires machine connector updates to use it)
- Add quick translate auto-set values, so sets can be used in other workflows
- Allow provider settings to be read via config, and config to read sections from `appsettings` if required
- Fix: nested blocks in translations weren't coming through
- Fix: localisation casing on the stages page

## 17.0.1 (`release/17.0.1`)

- Add missing migrations to remove obsolete columns
- Fix: config page now reloads when a setting is changed

## 17.0.0 (`release/17.0.0`)

First v17 release: ports the package to .NET 10 and Umbraco 17 with a UI refresh.

- Target .NET 10.0 and move to a `.slnx` solution
- UI refresh, including layout fixes and centering of the "no sets" message
- Remove the obsolete v13 assets package
- Update log levels on stop/fail states; downgrade some node creation errors to warnings or silent failures
- Fix: migration step rename error
