# Cloud-build the advanced NetHunter kernel (no Ubuntu on your PC)

This repo builds the **Pixel 4a 5G (bramble)** advanced NetHunter kernel on GitHub's **free
Ubuntu cloud runners** and gives you a flashable zip to download. Nothing Linux is installed on
your computer.

## What's inside
- `.github/workflows/build-kernel.yml` — the cloud build job
- `config/nethunter_extra.config`, `_plus.config`, `_ultra.config` — the locked feature set (3 tiers)

## How to run it (≈10 min of your time, ~40–60 min for the runner)

1. Create a **free GitHub account** if you don't have one: https://github.com/signup
2. Create a **new repository** (name it anything, e.g. `bramble-nethunter`). Keep it **Private** if you like.
3. Upload this whole `github-build` folder's contents to the repo:
   - Easiest: on the repo page → **Add file → Upload files** → drag in the `.github` folder and the
     `config` folder → **Commit**.
   - Or with git:
     ```bash
     cd "D:/Pixel 4A Firmware/github-build"
     git init && git add . && git commit -m "advanced nethunter kernel build"
     git branch -M main
     git remote add origin https://github.com/<you>/<repo>.git
     git push -u origin main
     ```
4. In the repo → **Actions** tab → enable workflows if prompted → pick
   **"Build Advanced NetHunter Kernel (bramble)"** → **Run workflow**.
5. When it finishes (green check), open the run → **Artifacts** → download
   **`Alynx-Advanced-Nethunter.zip`**.
6. Flash that zip via **Franco Kernel Manager**, then install/refresh the **FULL Kali chroot**.

## Expect 1–2 fix-up runs (normal for kernel builds)
First runs commonly stop on one of these — all quick to fix, and I can fix them for you from the
run's log:
- **Defconfig name**: the workflow has a step *"Show available defconfigs"*. If the real name
  isn't `redbull_defconfig`, tell me what it lists and I'll update the two `DEFCONFIG=` lines.
- **A 4.19 driver that won't compile** or a config symbol the tree doesn't define → I trim/patch it.
- **RTL8812AU wiring**: the out-of-tree driver may need Kconfig/Makefile hooks; if it breaks the
  build I'll gate it behind a patch or drop it for the first successful build.

**How to get me the log:** open the failed step in Actions → copy the red error lines → paste them
here. (If the repo is public, just paste the run URL and I can read it.) I'll push a corrected
workflow and you re-run.

## Notes
- The kernel base is LineageOS `android_kernel_google_redbull` branch `lineage-20` (Android 13, 4.19),
  the maintained bramble source.
- The feature set is exactly the locked 3 tiers (`../build/LOCKED-feature-set.md`).
- GitHub free tier gives plenty of Actions minutes for this; a build is ~40–60 min.
