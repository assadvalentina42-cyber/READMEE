# Planet Jumping

A single self-contained page (`index.html`, no build step). A short video plays as the preloader. A rounded "portal" window then tilts with the pointer, and clicking it zooms through the window into the next scene.

Open `index.html` in a browser to run it.

## Reusing the portal template

Everything you swap lives in the `TEMPLATE` config at the very top of `index.html`, inside the first `<script>`. The rest of the file reads from it, so you shouldn't need to touch the markup or the main script.

```js
const TEMPLATE = {
  logo:  { src: 'logo.svg', alt: 'Brand' },
  menu:  ['One', 'Two', 'Three'],   // sidebar labels; the current scene's name is highlighted
  scenes: [                          // travel order; the first scene is where the page starts
    {
      id: 'one', name: 'One', label: '[01]',
      arrival: 'intro.mp4',          // video played while zooming in (first scene: the preloader)
      focus: { x: 50, y: 50 },       // zoom/crop anchor, % of the media (0,0 = top-left)
      zoom: 1,                       // push-in amount on arrival (1 = none)
      notes: 'What should happen during this transition',
      facts: [['Label:', 'Value']]
    },
    {
      id: 'two', name: 'Two', label: '[02]',
      image: 'two.jpg',              // a still works too: no video needed
      focus: { x: 30, y: 70 }, zoom: 1.2,
      notes: 'Push into the lower-left subject'
    }
  ]
};
```

### Swapping images

- **Change a picture:** replace its `image` (or `arrival` video) path.
- **Re-aim the zoom:** set `focus` to the spot that should stay put as the zoom pushes in, as percentages of the picture. For example, `{ x: 80, y: 20 }` is near the top-right. The same point is used to crop the image to the screen, like CSS `object-position`.
- **Change the zoom strength:** `zoom` is the final scale, e.g. `1.15`. Use `1` for no push-in.
- **Add or remove a stop:** add or remove an entry in `scenes`. The portal always previews the next entry, and the last scene hides the portal.
- **Preview-only stop:** add `locked: true` to show a scene in the portal without letting people enter it.
- **`notes`:** for whoever edits the config. They are never shown on the page.

### Field reference

| Field | Required | Meaning |
| --- | --- | --- |
| `id` | yes | Unique key. Also set as `data-planet` on `.experience`, for per-scene CSS. |
| `name` | yes | Big title, and the "Next:" caption when this scene is upcoming. |
| `label` | no | Small number shown before the name in "Next:". |
| `arrival` | one of these two | Video played while travelling into the scene. On the first scene it is the ~3 s preloader. |
| `image` | one of these two | Still image used when there is no `arrival` video. |
| `background` | no | Optional video kept behind the page for this scene. |
| `focus` | no | `{x, y}` in %, default `{50, 50}`. |
| `zoom` | no | Default `1`. |
| `locked` | no | Preview in the portal only; cannot be entered. |
| `facts` | no | `[label, value]` rows shown bottom-right. |
| `notes` | no | Free-text transition notes for editors. |

If the first scene has no `arrival` video, the preloader countdown is skipped and its `image` is shown straight away.
