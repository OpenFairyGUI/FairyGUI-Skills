# Advanced Editor Features

Use this file for higher-risk FairyGUI editor features: transitions, masks, hit testing, custom data, custom properties, loaders, localization, import/export, and plugin-facing design.

Derived from:
- FairyGUI editor documentation for transitions, masks, hit testing, loaders, custom properties, custom data, localization, import/export, and plugins.
- Runtime integration patterns where editor feature choices directly affect code and performance.

## Transitions

Use when:
- Motion belongs to UI presentation and should be designer-tunable.
- Runtime code should trigger or hook animation rather than implement visual curves.
- A component needs intro/outro, feedback, list item effects, or number animation hooks.

Rules:
- Add labels for runtime hooks, value injection, retargeting, or partial playback.
- Add final keyframes when the UI must restore its initial state.
- Keep nested looping transitions deliberate because completion callbacks wait for nested content that can finish.
- Avoid changing component structure while editing a transition; transitions should animate existing objects.

## Masks

Use rectangular overflow masks when:
- The clipping area is rectangular.
- Performance and batching matter.

Use custom masks when:
- The visible area must be non-rectangular.
- The visual result justifies possible material/stencil/batching cost.

Rules:
- Test custom and reverse masks in the target runtime.
- Remember masked content can affect hit testing.
- Avoid custom masks in heavily repeated list items unless measured.

## Hit Testing and Click-Through

Use click-through when:
- A full-screen or overlay component should not block underlying UI in empty areas.
- Decorative panels contain non-touchable images/text and should pass input through.

Use custom hit tests when:
- The clickable area is not rectangular.
- A graph or pixel mask better represents the input area.

Rules:
- Pixel hit-test images must be in the same package and should not be loader content.
- Images and plain text are not touchable by default.
- Full-screen components can make `isTouchOnUI` always true unless click-through is handled.

## Loader Strategy

Use loaders for:
- Runtime-selected package URLs.
- External images, avatars, item icons, preview slots, or optional components.

Rules:
- Exclude editor-only test images from publish output when loaders only use them for preview.
- Keep loader slots named semantically because runtime code often owns their URL.
- Prefer static Image when the asset is fixed and package-owned.

## Custom Data

Use when:
- A component or object needs small runtime-readable metadata.
- The data is stable and belongs to the UI asset.

Avoid when:
- The data is large, localized, frequently updated, or owned by gameplay systems.

Rules:
- Treat custom data as a small bridge, not as a configuration database.
- Document expected format if runtime code parses it.

## Custom Properties

Use when:
- A reusable component needs design-time exposed knobs for nested child text/icon properties.
- Designers must configure nested content from the parent inspector.

Avoid when:
- Runtime code needs the same value. Runtime can read children directly; custom properties are editor-facing.

## Localization

Use when:
- Text is shipped in multiple languages.
- Text extraction and import/export should be repeatable.

Rules:
- Keep text as text fields where possible.
- Avoid baking localized strings into images unless typography/art direction requires it.
- Use scripted export when localization must be part of CI.

## Import and Export

Use when:
- UI assets need repeatable exchange with design tools or localization pipelines.
- Package output must be validated in CI.

Rules:
- Keep import/export settings documented near publish settings.
- Avoid manual one-off exports for assets that affect production builds.

## Plugin-Facing Design

Use when:
- Editor automation needs stable component names, custom data, or package conventions.
- Custom inspectors expose project-specific workflows.

Rules:
- Prefer undo-aware editor property APIs in plugins.
- Keep plugin-generated editor artifacts separate from runtime package output unless publishing is intentional.
- Name plugin UI packages and panels clearly so they do not conflict with product UI packages.
