# Plugin and Automation

Use this file for FairyGUI editor plugins, custom inspectors, menu extensions, Lua/TypeScript APIs, and scripted publishing/export.

Derived from:
- FairyGUI editor plugin documentation.
- Official Lua and TypeScript plugin examples, including menu extension, custom inspector, Lua API, and TypeScript API patterns.

## Plugin structure

- Keep each plugin in its own folder with a `package.json` and one main entry script.
- Use Lua for small editor automation and menu actions.
- Use TypeScript/Puerts when strong typing, custom panels, or complex editor API integration matters.
- Load plugin UI packages through `App.pluginManager.LoadUIPackage` when building custom panels or inspectors.

## Menu extensions

- Add menu entries through editor menu APIs, then remove them in the plugin destruction hook.
- Use stable command ids so reloads do not create duplicate entries.
- Keep menu actions short; delegate heavy operations to a function that can log progress and failure.

## Custom inspectors

- Implement custom inspectors by deriving from the editor plugin inspector API.
- Read current targets from `App.activeDoc.inspectingTarget` or `App.activeDoc.inspectingTargets`.
- Use `docElement.SetProperty(...)` when changing inspected objects so undo/redo works.
- Return true from update when the inspector should stay visible; return false when it should hide.
- Connect the inspector through the document factory with a clear condition for when it appears.

## Undo, selection, and document safety

- Prefer editor document/property APIs over direct object mutation for inspector-visible changes.
- Treat selection as volatile. Re-read active targets during update rather than caching long-lived object references.
- Handle mixed selection explicitly. If a property is not common to all targets, hide the inspector or show a neutral state.
- Avoid writing generated editor data into runtime package resources unless the operation is an explicit publish/export step.

## Scripted publish/export

- Use command-line `-script` hooks for CI tasks that need FairyGUI editor APIs.
- Use `PublishHandler` to publish packages from scripts.
- Use `ExportStringsHandler` for localization string export.
- Invoke the callback when an async handler finishes so batch mode can exit cleanly.

## Plugin review checklist

- Does the plugin clean up menus, event listeners, timers, and panels on destroy?
- Does it use undo-aware property setters for document changes?
- Does it avoid hard-coded absolute project paths?
- Does it fail with a visible log message when packages, panels, or selected objects are missing?
- Does it keep runtime artifacts and editor-only helper UI separate?
