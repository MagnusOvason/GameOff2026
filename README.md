# GameOff2026

Our entry for [GitHub Game Off 2026](https://itch.io/jam/game-off-2026).
A 2D pixel-art game made in Unity (URP 2D).

> Title and description will be updated once the theme is announced.

## Requirements

- **Unity**: the exact version in `ProjectSettings/ProjectVersion.txt`, with **Web Build Support**.
  Everyone must use the same version.
- **Git** and **Git LFS**
- **JetBrains Rider** (recommended)

## First-time setup

```powershell
# 1. Enable Git LFS (once per machine)
git lfs install

# 2. Clone
git clone https://github.com/<owner>/GameOff2026.git
cd GameOff2026

# 3. Set your identity for this repo only (use your GitHub noreply email if you like)
git config user.name  "Your Name"
git config user.email "you@example.com"

# 4. Configure Unity Smart Merge for scenes and prefabs
$ver   = (Select-String ProjectSettings\ProjectVersion.txt -Pattern 'm_EditorVersion: (.+)').Matches.Groups[1].Value
$merge = "C:/Program Files/Unity/Hub/Editor/$ver/Editor/Data/Tools/UnityYAMLMerge.exe"
Test-Path $merge   # must print True
git config merge.unityyamlmerge.name "Unity SmartMerge"
git config merge.unityyamlmerge.driver "'$merge' merge -h -p --force %O %B %A %A"
git config merge.unityyamlmerge.recursive binary
```

Then open the project from Unity Hub and:

- **Edit → Preferences → External Tools → Rider**, then **Regenerate project files**.
- Click the bug icon (bottom-right of the Editor) → **Debug mode**, so Rider breakpoints work.

### When the Unity version is upgraded

1. Install the new version in Unity Hub (with **Web Build Support**) and open the project with it.
2. **Regenerate project files** in Preferences → External Tools.
3. Re-run step 4 above. The Smart Merge path contains the Unity version.

## Workflow

- **GitHub Flow**: short-lived branches from `main`, merge via pull request. `main` must always build.
- **Conventional Commits**: `feat:`, `fix:`, `art:`, `audio:`, `chore:`, `refactor:`, `docs:`, `ci:`.
  Example: `feat(player): add dash ability`
- **Avoid scene conflicts**: don't edit the same scene at the same time. Work in prefabs where possible.
- Add every third-party asset to `CREDITS.md` **when you add it**.

## Builds

- Main target: **Web** (`Web - Desktop - Development` while developing, `Web - Desktop - Release` for uploads).
- A Windows build is added at the end as an extra download.

### Deploy to itch.io

Requires [butler](https://itchio.itch.io/butler) (`butler login` once), and admin access to the itch.io project.

1. In Unity: **File → Build Profiles → Web - Desktop - Release → Build**, output to `Builds/Web`.
2. Push:
   ```powershell
   butler push Builds\Web <itch-user>/gameoff2026:web --userversion 0.1.0
   ```
3. Windows build (end of jam): build to `Builds/Windows`, then
   `butler push Builds\Windows <itch-user>/gameoff2026:windows`

## License

- **Code**: MIT, see [LICENSE](LICENSE).
- **Original art and music**: CC BY 4.0, see [LICENSE-ASSETS.md](LICENSE-ASSETS.md).
- **Third-party assets**: their own licenses, see [CREDITS.md](CREDITS.md).
