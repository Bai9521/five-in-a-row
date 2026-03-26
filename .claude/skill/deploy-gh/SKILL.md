---
name: deploy-gh
description: Deploy current project to GitHub Pages. Use when user wants to quickly deploy a static website to GitHub Pages.
license: MIT
compatibility: Requires gh CLI and git.
metadata:
  author: Claude
  version: "1.0"
---

Deploy the current project to GitHub Pages.

---

**Input**: GitHub repository name and description (can be asked if not provided).

**Steps**

1. **Initialize git repository (if not exists)**
   ```bash
   git status
   ```
   If not a git repo:
   ```bash
   git init
   ```

2. **Check if index.html exists**
   - If not, ask user to rename the main HTML file to `index.html`
   - GitHub Pages requires `index.html` at root

3. **Create initial commit**
   ```bash
   git add . && git commit -m "Initial commit"
   ```

4. **Ask for repository details if not provided**
   - Repository name (e.g., `my-game`)
   - Description (e.g., `五子棋游戏`)

5. **Create GitHub repository and push**
   ```bash
   gh repo create [名称] --public --source=. --description "[描述]" --push
   ```

6. **Enable GitHub Pages**
   Get the GitHub username:
   ```bash
   gh auth status
   ```
   Then enable Pages:
   ```bash
   gh api repos/[owner]/[名称]/pages -X POST -f "source[branch]=master"
   ```

7. **Wait for build and verify**
   Poll the status:
   ```bash
   gh api repos/[owner]/[名称]/pages -X GET
   ```
   Wait until status is "built" (may take 1-2 minutes)

8. **Display the deployed URL**
   Format: `https://[username].github.io/[repo-name]/`

**Output**

Show the user:
- Repository URL: `https://github.com/[owner]/[name]`
- Live site URL: `https://[username].github.io/[name]/`
- Note that it may take a few minutes for GitHub Pages to build

**Notes**

- The main HTML file must be named `index.html`
- Only works with static HTML/CSS/JS projects
- Requires user to be logged into gh CLI (`gh auth status`)