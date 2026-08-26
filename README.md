# Macroslop Binbows

The public site for Macroslop Binbows' Windows tools, served from GitHub Pages:

<https://skeletonlogic.github.io/MacroslopBinbows/>

It is one static page with no build step and no dependencies. Everything in
this repository is the site; the applications themselves live elsewhere and are
distributed as **release assets**, not as files in this repo.

```
index.html          the page
assets/logo.png     wordmark
assets/backdrop.jpg sky
assets/video/       one 30s silent promo loop + poster per product
.nojekyll           serve the files as they are, no Jekyll pass
```

## Downloads

Both download buttons point at:

```
https://github.com/SkeletonLogic/MacroslopBinbows/releases/latest/download/<asset>
```

which always resolves to the newest published release, so the page never needs
editing when a version ships. The asset filenames must stay stable:

| Product | Asset | Size |
|---|---|---|
| Macroslop Snipper | `MacroslopSnipper.exe` | ~65 MB |
| Macroslop SoundUFer | `MacroslopSoundUFer.exe` | ~700 KB |

Each product is a single self-contained executable. Snipper carries its own
Python, ffmpeg and numpy; SoundUFer embeds `SoundUFerClone.exe` and
`SoundUFerDriverSetup.exe` as resources. Nothing else needs to ship beside them.

### Editions

Downloads are the **Free** edition, and that needs no separate build. Edition is
per-user state read from
`HKCU\SOFTWARE\Macroslop Binbows\<Product>` → `Edition`; anything that is not
the string `Premium` — including the usual case of the value not existing —
means Free. Nothing in either product ever writes `Free`, so a fresh install
simply starts there. One executable serves both editions.

### The SmartScreen warning

Neither executable is code-signed, so Windows shows a blue *"Windows protected
your PC"* panel on first run and hides the Run button behind **More info**. Say
so in the release notes, in these words:

> Windows will show a blue "Windows protected your PC" screen the first time.
> Click **More info**, then **Run anyway**. It appears once; after that the app
> opens normally.

It is a reputation warning, not a malware finding. Only an Authenticode
signature removes it — see `MacroslopSnipper/DISTRIBUTING.md` in the source
tree.

## Publishing

This page and the Claude artifact version of it (`../website/index.html`) were
generated together from one script, but that script was not kept. They are now
two hand-maintained copies of the same design: a change to one has to be made
in the other. The differences between them are deliberate and worth preserving
— see `../website/README.md`.

## Promo loops

1280×720, H.264, 20 fps, CRF 22, silent, `+faststart`, ~1.7 MB each — the
highest-quality copies that exist, since the renderer composes at 960×540 and
upscales. They stream with range requests rather than being inlined.

The page attaches a loop only once its card is within 600 px of the viewport,
plays it only while it is on screen, and stops everything when the tab is
backgrounded. Under `prefers-reduced-motion: reduce` or `Save-Data` the video is
never fetched and the poster stands in.

There are deliberately **no playback controls**: no `controls` attribute,
`pointer-events:none` on the video, a gloss layer above it absorbing clicks,
`contextmenu` cancelled, plus `disablepictureinpicture`, `disableremoteplayback`
and `controlslist="nodownload noplaybackrate noremoteplayback"`.
