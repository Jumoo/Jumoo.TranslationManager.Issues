# ChangeLog
List of changes for releases of Translation Manager v8 - for Umbraco.

Detailed release note at https://jumoo.co.uk/translate/releases 

## v8.7.10 - 2022-04-27
 - Fix: Provider settings values return false when they are blank
 - Fix: Allow Passthrough provider to not change values when they already exist on target
 - Fix: Improve the error logging if a file upload files in the XLIFF provider #31
 - Fix: Improves xliff error checking / display #31
 - Fix: Elements containing only spanning codes are treated as blank values when they are not #27
 - Fix: Track version number when using loopback sets
 - Fix: Ensure 'title' attributes are split out of translation correctly when embedded in RTE/Grid element

## v8.7.9 - 2021-01-10 
- Ignore metadata with it doesn't have an ID value
- Json fixes for doctypegrideditor values

## v8.7.8 - 2021-11-10
- option to alter the list of inline and block elements used when splitting html in xliff 1.2 files

## v8.7.7 - 2021-10-26
- Anchor tags in Xliff 2.0 contain a empty subType value - when the should not
- In certain circumstances html `em` tags get treated as xliff 2.0 `em` tags - resulting in some tags not being split in the xliff.

## v8.7.6 - 2021-10-13
- Fix for "Error one or more nodes could not be created" message - when using a loopback set.
- Spelling fixes in language file
- better logging for errors when you attempt to send languages to multiple sets.

## v8.7.5 - 2021-09-02
Pathc release Xliff Splitting & Spellings 

- Add extra data to groups when splitting xliff 1.2 groups. (stops loss of class attributes on embedded html markup)
- typo's and spellings
- Performance improcement on sort/move operations.

## v8.7.4 - 2021-08-18
Patch release fixing Json Nested content naming 

- Escape Json in NestedContent Property values (don't store directly as a JSON Object)

## v8.7.3 - 2021-08-13
Updates to Xliff Splitting and job names 

- Added JobId/JobKey to list of replacements on xliff file name.
- Fix - Non-media paths not saving/loading (v8.7.2 regression)

## v8.7.2 - 2021-08-05
Patch release azure media storate fixed

- Fix - preStrip media path if present, so Umbraco Cloud URLs don't have double  media
- Update xliff 1.2 spitting to split text if its html.

## v8.7.1 - 2021-07-15 
Patch release - fix some blob / regex edge cases 

### Fixes 
 - [#12](https://github.com/Jumoo/Jumoo.TranslationManager.Issues/issues/12) Media paths incorrect if blob doesn't use VirtualPathProvider
 - RegEx fix for some properties that don't set path
 - Improved json detection in multiple list entries. 

## v8.7.0 - 2021-07-07
Blob storage support release 

 - Support for blob storage when xliff files are saved in media folder
 - RegEx support for 'properties' element in set config 
 - custom inline xliff splitting (via config)

## v8.6.2 - 2021-06-10
Patch Release - Fix false blank Target report

### Fixes
 - [#10](https://github.com/Jumoo/Jumoo.TranslationManager.Issues/issues/10) Xliff Target may report as blank if it starts wih or only contains a spanning code.

## v8.6.1 - 2021-04-12
Patch Release: Fix Dictionart permissions check and UI issue.

### Fixes
 - [#7](https://github.com/Jumoo/Jumoo.TranslationManager.Issues/issues/7) Approving Dictionary items - results in object not found error.
 - [#8](https://github.com/Jumoo/Jumoo.TranslationManager.Issues/issues/8) User can still see "Save and Review" option when they don't have permissions.

## v8.6.0 - 2021-04-09
Minor release :  _(Reason Missing target checking changes default behavior of creation flow)_

### Fixes
 - [#1](https://github.com/Jumoo/Jumoo.TranslationManager.Issues/issues/1) blank dicrionary/content items causes serialization failure
 - [#2](https://github.com/Jumoo/Jumoo.TranslationManager.Issues/issues/2) "Check github" button points to wrong repo.
 - [#4](https://github.com/Jumoo/Jumoo.TranslationManager.Issues/issues/4) View button for target doesn't show variant language on loopback
 - [#5](https://github.com/Jumoo/Jumoo.TranslationManager.Issues/issues/5) Save and Publish and Save and Approve buttons don't obey content privileges
 - [#6](https://github.com/Jumoo/Jumoo.TranslationManager.Issues/issues/5) rouge Licence is correct but not for this domain message

### Features
 - [#3](https://github.com/Jumoo/Jumoo.TranslationManager.Issues/issues/3) Warn when target nodes cannot be found because pages are not linked

## v8.5.3 - 2021-03-30

### Fixes 

- multilingual view button fix
- macro id fix. 

## v8.5.2 - 2021-03-10

### Fixes
- UI Improvements
  - no cancel during provider process
  - improved refresh of pending / submitted after job
  - better selection checkbox UI

### Features
- XLIFF 1.2 Splitting support

## v8.5.0 - 2021-01-27

### Fixes
- licence dialog fix

### Features
- Branch translation dashboard
- Job history
- improved error dialogs
- agency licencing model


## v8.4.3 - 2020-12-17

### Fixes 
- stop change to global serialization settings from breaking existing jobs

## v8.4.2 - 2020-12-02

### Fixes
- repeatable strings inside grid editors are blank
- fix us to show diffrence between translated / non-translated values in a job

### Features
- improved confirmation boxes for job cancellation

## v8.4.1 - 2020-10-08

### Fixes 
- `<pc>` values not extracted correctly in xliff.
- Content app - don't count words till in focus

### Features
- Don't send blank values in xliff

## v8.4.0 - 2020-09-15

### Features 
- Blocklist editor support
- UI Improvements for Umbraco 8.7+

## v8.3.2 - 2020-08-27

### Fixes
- Set Error when master or target has been deleted

## v8.3.1 - 2020-07-29

### Fixes
- creating jobs from content app has 0 nodes

## v8.3.0 - 2020-07-24

### Features
- Custom editor mapping via config
- Link updater mapping config support

## v8.2.12 - 2020-06-13

### Fixes 
- Additonal spaces in html while merging XLIFF
- SignalR Hub reset can cause load fail
- Trailing slash on API call can cause rewrite rules to trigger

### Features
- Dashboard list view

## v8.2.10 - 2020-05-10

### Fixes
- ServerVariables JS values serialization issues

### Feature
- Provider validation to allow halt in UI progress.

## v8.2.9 - 2020-04-23

### Fixes
- Global serializer settings corrupt some models

## v8.2.8 - 2020-04-01

### Fixes
- Non supported HTML elements inside XLIFF elements not seralized correctly
- Blank target copy doesn't copy sub elements

## v8.2.7 - 2020-03-13

### Fixes
- Copy to target fails when source is blank
- Links/Tags not translated on import
- Spanning code in ignorable elements not imported
- Word count on content app is inaccurate
- create button enabled when no provider is selected

## v8.2.6 - 2020-02-27

### Fixes
- Empty dictionart items are not synced
- XLIFF spanning elements do not serialize correctly
- XLIFF empty anchor tags do not get reimported
- Job creation count out by one.

## v8.2.5 - 2020-02-03

### Fixes
- Property order changes during translation can cause issues
- Grid value sort order stored as int

## v8.2.4 - 2020-01-28

### Fixes
- Macro parameters changing during translation job can cause issues

### Features
- UI Tweak create dialog
- Autoselect single language
- grammar & spelling

## v8.2.3 - 2019-12-17

### Fixes
- Saving nodes that don't vary by variant but are in a loopback set.

## v8.2.2 - 2019-11-29

### Fixes
- connector settings converted to bool

### Features
- File name templating for Xliff
- better warnings on dictionary jobs

## v8.2.1 - 2019-11-29

### Fixes
- HTML splitting 
  - html comments
  - Div containing media
- JSON Nested content fail on import
- Content locking

## v8.2.0 - 2019-10-29

### Fixes
- `a` tags not always rendered
- menu syncing fix
- dialog buttons disabled while busy

### Features
- convert culture tacking to ISO code
- Dictionary Send to translate 
- extra html tag splitting in xliff
- improved job listings including job author
- select all language option
- improved job name formatting options
- xliff 2.1 serializer (experimental)

## v8.1.5 - 2019-09-16

### Fixes
- Only publish updating culture when approving multivariant content
- Improve Xliff merging when split by sentance

### Features
- Linkresolver support for links outside of current set
- 
## v8.1.4 - 2019-09-04

### Fixes
- Text inside anchor tag not translated
- jobs do open when clicked in umbraco 8.1.4
- throw error be translate if node doesn't vary by culture
- dialog bounce fix

### Features
- DotypeGridEditor support
- UI: Language dropdown when 10+ languages in set

## v8.1.3 - 2019-07-31

### Fixes
- Variant loop back relation issues
- unpublished node fails to translate (redux)
- job creation errors disapear

### Features
- variant clone button in content app 
- new orphaned sets health check
- XLIFF error message improvments

## v8.1.2 - 2019-07-26

### Fixes
- status changes trigger events

### Features
- unpublished nodes fail to translate

## v8.1.1 - 2019-07-09

### Fixes
- Missing notifications email templates

### Features
- per set notifications
- searchable trees
- My job view
- Job grouping capability for providers

## v8.1.0 - 2019-06-16

### Features
- Automatic translation approval
- improved status indicators
- NEW: Google translator connector
- NEW: Passthrough translator connector

### Changes
- Removal of Automapper dependency


## v8.0.1 - 2019-05-16

### Fixes
- Edit button opens up correct culture
- child nodes erro when using variants

## v8.0.0 - 2019-05-14

- Initial release
