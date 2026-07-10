# Changelog

All notable changes to Translation Manager for Umbraco v18, by release tag.

## 18.0.1 (`release/v18.0.1`)

Fix a duplicate custom-element registration that could appear after upgrading, caused by the backoffice client being loaded twice from browser cache.

- Fix: align the `@jumoo/translate` import-map entry with the versioned backoffice entrypoint URL, so a stale cached copy can no longer load alongside the fresh build and register custom elements twice (e.g. `jumoo-tm-settings-connector-menu has already been used with this registry`)

## 18.0.0 (`release/v18.0.0`)

Umbraco v18 release, adding support for translating Library elements (`IElement` / `IElementContainer`).
