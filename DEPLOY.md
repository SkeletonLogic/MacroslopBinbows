# Deploying

The repository is committed locally and ready to push. Everything below needs
your GitHub credentials, so it is left for you to run — nothing has been sent
to GitHub yet.

## 1. Create the repository and push

`gh` is not installed on this machine. Either install it:

```bash
winget install --id GitHub.cli --source winget
```

then, from the `site` folder:

```bash
gh auth login
```

```bash
gh repo create MacroslopBinbows --public --source=. --remote=origin --push
```

Or, without `gh`: create an empty public repository named `MacroslopBinbows` at
<https://github.com/new> (no README, no .gitignore, no licence), then:

```bash
git remote add origin https://github.com/SkeletonLogic/MacroslopBinbows.git
```

```bash
git push -u origin main
```

## 2. Turn on Pages

```bash
gh api -X POST repos/SkeletonLogic/MacroslopBinbows/pages -f "source[branch]=main" -f "source[path]=/"
```

Or in the browser: **Settings → Pages → Source: Deploy from a branch → main / (root)**.

The site appears at <https://skeletonlogic.github.io/MacroslopBinbows/> within
a minute or two.

## 3. Publish the release the download buttons point at

The buttons resolve `releases/latest/download/<asset>`, so they stay broken
until a release exists with **exactly** these asset names:

```bash
gh release create v1.0 --title "Macroslop Binbows v1.0" --notes-file RELEASE_BODY.md "C:/Users/hawth/Documents/Macroslop Binbows Apps/MacroslopSnipper/dist/MacroslopSnipper.exe" "C:/Users/hawth/Documents/Macroslop Binbows Apps/MacroslopSoundUFer/MacroslopSoundUFer.exe"
```

Or drag those two files onto a new release at
<https://github.com/SkeletonLogic/MacroslopBinbows/releases/new>, tagged `v1.0`.

Do not rename the assets. `MacroslopSnipper.exe` and `MacroslopSoundUFer.exe`
are what the page asks for, and `latest/download` matches on filename.

Suggested release body — save as `RELEASE_BODY.md`, or paste it in:

> **Macroslop Snipper** — free-form, rectangle, window and full-screen snips,
> optional delay, GIF and MP4 recording at your own frame rate and duration, and
> an editor with pen, highlighter and eraser.
>
> **Macroslop SoundUFer** — drag an application onto a playback device to move
> its audio, Ctrl+click to clone it to a second device, Ctrl+click a device to
> mirror everything to two outputs at once. Low-latency duplicates with drift
> correction.
>
> Both are single 64-bit executables. No installer, no runtime to fetch, no
> network access. Windows 10 or 11, 64-bit. Standard user rights — neither asks
> for administrator.
>
> Windows will show a blue "Windows protected your PC" screen the first time.
> Click **More info**, then **Run anyway**. It appears once; after that the app
> opens normally. The executables are not code-signed, so Windows has no
> reputation for them yet — the warning says nothing about the app being
> harmful.

## 4. Check

```bash
curl -sIL https://github.com/SkeletonLogic/MacroslopBinbows/releases/latest/download/MacroslopSnipper.exe | grep -E "^HTTP|location"
```

A `200` at the end means the page's buttons work.
