# GitHub + jsDelivr Image Host Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a public `mcmod-images` GitHub repository with a maintainable image directory structure and verify that a committed test asset is served by jsDelivr.

**Architecture:** GitHub stores ordinary Git objects in a public repository; jsDelivr reads the repository and exposes immutable tag URLs plus a convenient `@main` URL. Text documentation and repository rules live beside categorized image folders, while local Git history and original images provide a migration path.

**Tech Stack:** Git, GitHub web UI, jsDelivr GitHub CDN, Markdown, SVG test asset

---

### Task 1: Create repository scaffolding

**Files:**
- Create: `README.md`
- Create: `.gitattributes`
- Create: `.gitignore`
- Create: `images/backgrounds/.gitkeep`
- Create: `images/mods/.gitkeep`
- Create: `images/entities/.gitkeep`
- Create: `images/ui/jsdelivr-check.svg`

- [ ] **Step 1: Create the repository rules files**

Create `.gitattributes` with:

```gitattributes
* text=auto
*.jpg binary
*.jpeg binary
*.png binary
*.webp binary
*.gif binary
*.svg text eol=lf
```

Create `.gitignore` with:

```gitignore
Thumbs.db
Desktop.ini
.DS_Store
*.tmp
```

- [ ] **Step 2: Create the category directories**

Create empty marker files at:

```text
images/backgrounds/.gitkeep
images/mods/.gitkeep
images/entities/.gitkeep
```

- [ ] **Step 3: Create a deterministic CDN test asset**

Create `images/ui/jsdelivr-check.svg` with:

```svg
<svg xmlns="http://www.w3.org/2000/svg" width="320" height="180" viewBox="0 0 320 180">
  <rect width="320" height="180" fill="#151b23"/>
  <path d="M62 94l24 24 58-64" fill="none" stroke="#3fb950" stroke-width="14" stroke-linecap="round" stroke-linejoin="round"/>
  <text x="164" y="92" fill="#f0f6fc" font-family="Arial, sans-serif" font-size="22" text-anchor="middle">jsDelivr ready</text>
  <text x="164" y="122" fill="#8b949e" font-family="Arial, sans-serif" font-size="14" text-anchor="middle">mcmod-images</text>
</svg>
```

- [ ] **Step 4: Create repository documentation**

Create `README.md` documenting:

```markdown
# mcmod-images

Public static image assets for the MC China Edition Mod Wiki.

## Direct URL

`https://cdn.jsdelivr.net/gh/${GITHUB_USER}/mcmod-images@main/images/ui/jsdelivr-check.svg`

Replace `${GITHUB_USER}` with the repository owner's GitHub username.

## Directories

- `images/backgrounds/`: site and page backgrounds
- `images/mods/`: mod posters and screenshots
- `images/entities/`: entity illustrations and screenshots
- `images/ui/`: icons and interface images

## Rules

- Use lowercase ASCII file names without spaces.
- Prefer WebP, PNG, or JPEG and keep each image below 5 MB.
- Do not use Git LFS.
- Do not overwrite a published image path; add a version suffix instead.
- Keep the original image outside GitHub as a backup.
```

- [ ] **Step 5: Verify the local structure**

Run:

```powershell
Get-ChildItem -LiteralPath . -Recurse -Force | Select-Object FullName,Length
git status --short
```

Expected: the seven scaffold files are present and listed as untracked before commit.

- [ ] **Step 6: Commit the scaffolding**

Run:

```powershell
git add README.md .gitattributes .gitignore images
git commit -m "feat: scaffold jsDelivr image repository"
```

Expected: one commit containing repository documentation, rules, directory markers, and the SVG test asset.

### Task 2: Publish the GitHub repository

**Files:**
- Upload: all committed files in the local repository

- [ ] **Step 1: Confirm the authenticated GitHub identity**

Open `https://github.com/` in the authenticated browser and read the visible account menu. Store the displayed login name as `GITHUB_USER` for URL construction; do not inspect cookies, tokens, passwords, or local storage.

- [ ] **Step 2: Create the public repository**

In GitHub's new repository form set:

```text
Repository name: mcmod-images
Description: Public static image assets for the MC China Edition Mod Wiki
Visibility: Public
Initialize with README: No
Add .gitignore: None
License: None
```

Expected: GitHub opens `https://github.com/${GITHUB_USER}/mcmod-images` and shows an empty public repository.

- [ ] **Step 3: Publish the local commits**

Add the HTTPS remote and push:

```powershell
git remote add origin https://github.com/${GITHUB_USER}/mcmod-images.git
git push -u origin main
```

If Git Credential Manager requests authorization, use the already-approved browser authorization flow without entering or exposing a password or personal access token in terminal output.

Expected: `main` tracks `origin/main`, and the repository web page lists `README.md`, `.gitattributes`, `.gitignore`, `images/`, and `docs/`.

### Task 3: Verify GitHub and jsDelivr delivery

**Files:**
- Verify: `images/ui/jsdelivr-check.svg`

- [ ] **Step 1: Verify the GitHub raw resource**

Request:

```text
https://raw.githubusercontent.com/${GITHUB_USER}/mcmod-images/main/images/ui/jsdelivr-check.svg
```

Expected: HTTP 200 and `Content-Type: image/svg+xml` or another SVG-compatible content type.

- [ ] **Step 2: Verify the jsDelivr resource**

Request:

```text
https://cdn.jsdelivr.net/gh/${GITHUB_USER}/mcmod-images@main/images/ui/jsdelivr-check.svg
```

Expected: HTTP 200, non-empty body, and the rendered text `jsDelivr ready`.

- [ ] **Step 3: Record the resolved URLs in README**

Replace the README URL template with the verified concrete URL containing the authenticated GitHub username, then run:

```powershell
git add README.md
git commit -m "docs: record verified jsDelivr URL"
git push
```

Expected: README contains a clickable, verified CDN URL.

- [ ] **Step 4: Final verification**

Run:

```powershell
git status --short
git log --oneline -3
```

Expected: clean working tree and commits for the design, scaffolding, and verified CDN documentation.

