# Changelog

All notable changes to Translation Manager for Umbraco v18, by release tag.

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
