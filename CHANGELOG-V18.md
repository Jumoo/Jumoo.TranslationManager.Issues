# Changelog

All notable changes to Translation Manager for Umbraco v18, by release tag.

## 18.3.2 (`v18.3.2`)

A bug-fix release, bringing the fixes from 17.9.2 and 17.9.3 to the v18 line. If you run a website-only (delivery) tier, publish translations on culture-variant pages that use blocks, use a custom connector, or run Translation Manager alongside uSync.Complete, we recommend upgrading.

- Fix: on a website-only boot (a site running without the Umbraco backoffice, common in load-balanced setups), several parts of Translation Manager (the link updater, the property copier, and Xliff file handling) still tried to register services that depend on the backoffice, and Umbraco's own health-check discovery found Translation Manager's health checks regardless of our own checks. Both are now skipped correctly, so a website-only site starts cleanly
- Fix: on a fresh install, approving and publishing a translation could fail with "You do not have publish permissions for one or more target node", even for an administrator, until the site was restarted. The new approval permissions now take effect straight away, on every server in a load-balanced setup
- Fix: after publishing a translation of a culture-variant page that contains a Block List (or a rich text editor with blocks), the default language could stay flagged as having unpublished changes. Translated block content is now saved exactly as Umbraco saves it on publish. One case remains: a rich text editor that doesn't vary by culture but contains blocks that do can still show pending changes, because of how Umbraco itself orders those values on publish
- Fix: a custom connector that imports translations directly, rather than through the backoffice, stopped saving them to Umbraco in 18.3.0. It works as it did before again, and the backoffice still won't let you approve pages that are waiting on a translation
- Fix: saving translation processing settings could fail with a 400 error ("unrecognized type discriminator id") when Translation Manager is installed alongside another Jumoo package, such as uSync.Complete, that uses a different version of the shared processing library. Existing settings carry on working unchanged
- Fix: the pending items list now shows the most recently updated items first, so an item you've just re-triggered moves back to the top rather than staying buried under newer ones ([#84](https://github.com/Jumoo/Jumoo.TranslationManager.Issues/issues/84))
- Fix: in a translation job, selecting every item on the current page is now a "Select page" button next to "Select All" and "Clear Selection", and "Select All" makes clear that it selects items on every page of the job
- Build: this package now ships with a matching, current version of its Xliff serialization component rather than an older one pulled in indirectly through a connector
- Build: the `@jumoo/translate` npm package (for developers building backoffice extensions) now comes from npmjs.org, and its version matches the NuGet package

## 18.3.1 (`v18.3.1`)

A performance fix for large multi-site, multi-culture installs.

- Fix: browsing the content tree in the backoffice could make the site progressively slower over the course of a day — sometimes badly enough to freeze the backoffice and lose editor work — on a site with a translation set spanning several sites and cultures. Each node loaded in the tree triggered a lookup whose response kept growing, because a duplicate site entry was being added to that set's cached data every time and never cleared. Fixed by comparing against the right culture when checking whether an entry already existed, and by making sure that kind of per-request lookup can no longer write back into the shared cache at all, regardless of what changes there in future

## 18.3.0 (`v18.3.0`)

A global Glossary for keeping terminology consistent across translations, granular permissions for Glossary and Translation Memory, and a round of reliability fixes.

- Add: Glossary — a global list of terminology you control per language pair, importable/exportable as CSV, so translations use your preferred wording instead of the connector's own choice
- Add: `jumoo-view-glossary` and `jumoo-view-translation-memory` permissions, so access to these areas can be granted independently of general backoffice access
- Fix: a partially-approved translation job could show the wrong status, or let a node be approved before it was actually ready
- Fix: jobs with nothing left to translate now show a normal "nothing to do" message instead of an error
- Fix: the create-job dialog now requires a connector to be selected before you can continue
- Fix: duplicate labels on the glossary on/off toggles
- Improve: translation requests now retry only on genuine transient network failures, reducing unnecessary retries

## 18.2.1 (`v18.2.1`)

Fixes carried forward from the v17 line: a first-install translation set guess, a connector auto-select fix, and a licence-check identification header.

- Fix: 18.2.0's build could pull in unused ASP.NET Core dependencies that broke installation on some sites; the build pipeline now pins an exact .NET SDK version and verifies its full dependency graph on every release, so this can't recur unnoticed
- Add: on a fresh install with no translation set configured yet, Translation Manager now guesses and selects a starter set automatically
- Fix: the create-job dialog now auto-selects the connector when a translation set has only one available, instead of leaving it unselected (fixes #94)
- Fix: licence check requests now identify themselves with a Translation Manager user agent header instead of .NET's default

## 18.2.0 (`v18.2.0`)

A bulk translation dashboard for the Content section, and access-level aware connector settings.

- Add: bulk translation dashboard in the Content section — pick a page tree and language, review what's pending, and send it all to translation in one go
- Improve: connector settings lookups now respect the current user's Settings-section access, so job creation only requests the detail it actually needs
- Fix: a manual job check failure now shows one clear error instead of a generic message

## 18.1.2 (`release/v18.1.2`)

Translation memory correctness fixes, a job-cancel crash guard, and a provider-selection validation fix.

- Fix: approved translation memory could silently revert to pending when shared source text was re-translated by another node, and editing a single value on a node incorrectly deleted memory for every other property on that node; approval is now monotonic and edit-invalidation only removes memory rows that no longer match the node's current source text
- Fix: guard against a `NullReferenceException` in `TranslationJobService.Cancel` and `MachineConnectorBase.SubmitInternal` when acting on a job whose `Nodes` collection isn't loaded; cancel now self-heals by reloading nodes from the database
- Fix: the choose-provider step could let job creation proceed before an async default-connector resolution finished, saving a job with no `providerKey` when a set has no default connector
- Fix: stop log flooding from benign no-set-match cases during node creation on invariant doctypes and culture-only saves
- Build: bump `Jumoo.TranslationManager.Microsoft` to 18.1.0, `Jumoo.TranslationManager.AI` to 18.1.1, and `Jumoo.Processing` to 18.1.0

## 18.1.1 (`release/v18.1.1`)

Fix a rich text property crash caused by empty markup round-tripping as `null`.

- Fix: an RTE property with empty markup (e.g. a block-only rich text value) could come back from translation as `{"markup": null, "blocks": ...}` instead of `{"markup": "", "blocks": ...}`, which made the node unopenable in the backoffice

## 18.1.0 (`release/v18.1.0`)

Translate in place, granular translation permissions, an editable rich-text HTML view, and set-level connector locking.

- Feat: "Translate in place" — translate a node's own content into another language with no configured Translation Set, overwriting it in place (works for both culture-varying and invariant doctypes); off by default behind a new `inPlaceTranslation` setting (fixes #95)
- Feat: split "publish" out of the existing translation-approve permission, so a translator can be given approve rights without also being able to publish the result back to the site (fixes #101)
- Fix: mutating job/node/set/memory/connector endpoints previously only required generic backoffice access — they now require the appropriate translation permission. **Behaviour change on upgrade:** a group holding only `jumoo-send-to-translation` can no longer archive, remove or reset jobs, or edit sets
- Feat: edit HTML (`htmlControl`) translation values with a Tiptap rich text editor instead of a raw-markup textarea
- Feat: "Lock connector" option on translation sets — a set's default connector is now a starting point rather than the only option; tick "Lock connector" to keep the old forced/read-only behaviour (existing sets default to locked, so behaviour is unchanged until someone opts out)
- Change: rework the settings page into a two-column layout, rename "Translate on save" to "Pending Translations", and surface the translate-in-place toggle
- Fix: approved node memories were incorrectly deleted in some cases
- Build: bump the Xliff connector dependency to 18.0.1; other bundled connectors remain at 18.0.0

## 18.0.1 (`release/v18.0.1`)

Fix a duplicate custom-element registration that could appear after upgrading, caused by the backoffice client being loaded twice from browser cache.

- Fix: align the `@jumoo/translate` import-map entry with the versioned backoffice entrypoint URL, so a stale cached copy can no longer load alongside the fresh build and register custom elements twice (e.g. `jumoo-tm-settings-connector-menu has already been used with this registry`)

## 18.0.0 (`release/v18.0.0`)

Umbraco v18 release, adding support for translating Library elements (`IElement` / `IElementContainer`).
