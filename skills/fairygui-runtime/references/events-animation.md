# Events and Animation

Use this file for input events, bubbling/capture, click item events, transitions, hooks, tweens, and animation-related troubleshooting.

Derived from:
- FairyGUI event-system and transition documentation.
- Official Unity transition examples and Cocos Creator menu/event wiring examples.

## Events

- Prefer FairyGUI event APIs on `GObject`/`GComponent` for UI interaction; do not bypass custom hit testing with host-engine touch APIs unless necessary.
- Use `onClick` helpers for simple clicks.
- Use event context when sender, initiator, input data, custom data, or propagation control matters.
- Stop propagation only when parent components should not react to a child input.
- Use touch capture when a drag/touch-end must continue after the pointer leaves the original object.

Unity C#:

```csharp
view.GetChild("btn").onClick.Add(() => { /* handle click */ });
list.onClickItem.Add(OnClickItem);
```

Cocos Creator:

```ts
this._view.getChild("n1").onClick(() => this.startDemo(BasicDemo));
list.on(fgui.Event.CLICK_ITEM, this.onClickItem, this);
```

LayaAir:

```ts
button.onClick(this, this.onClick);
list.on(fgui.Events.CLICK_ITEM, this, this.onClickItem);
```

## Event data rules

- Store lightweight custom data on `GObject.data` when a handler needs row or model identity.
- Prefer closure/lambda capture for short local handlers.
- Remove or dispose views that own events; otherwise long-lived roots can keep handler targets alive.

## Transitions

- Get transitions by name from the component that owns them.
- Play transitions in response to lifecycle or event triggers.
- Use callbacks to restore UI state, remove temporary objects, or resume blocked interactions.
- Use frame labels for hooks and dynamic values.

Unity transition pattern:

```csharp
Transition t = target.GetTransition("t0");
t.Play(() => {
    GRoot.inst.RemoveChild(target);
});
```

Hook pattern:

```csharp
_g5.GetTransition("t0").SetHook("play_num_now", PlayNum);
```

## Transition state rules

- Transition playback leaves the component at the final frame unless the transition restores state.
- Repeated transitions can behave unexpectedly when the first keyframe leaves a property unchecked.
- Retarget only while stopped.
- In Unity, transitions normally ignore `Time.timeScale`; opt in if game pause should affect UI animation.
- If batching order breaks during a transition that moves/resizes overlapped content, adjust animation design first, then consider invalidating batching every frame.

## Tween rules

- Use FairyGUI/GTween for UI-owned numeric interpolation so it stays consistent with FairyGUI lifecycle.
- Keep tweens attached to a meaningful target when possible.
- For number roll-up effects, update text in `OnUpdate` and set the final value explicitly if precision matters.
