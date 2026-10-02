# Hello, world

A one-page site: a greeting, a language switcher, and your local time. `index.html` is the whole page. Open it in a browser, or publish it with GitHub Pages.

## Preview locally

From this folder:

```bash
python3 -m http.server 8080
```

Then open [http://localhost:8080](http://localhost:8080).

## Deploy to GitHub Pages

GitHub Pages serves this folder as a static site. There is no build step. The homepage is `index.html` at the root of the repository.

Pages on the free plan is available for public repositories. Private repositories need a paid GitHub plan.

### 1. Create a repository

1. On GitHub, create a new repository. Any name works, for example `hello-world`.
2. Leave it empty. Do not add a README, license, or .gitignore there. This folder already has a README.

### 2. Push this folder

In a terminal, from this folder, replace `USER` and `REPO` with your GitHub username and repository name:

```bash
git init
git add .
git commit -m "Add the hello world site"
git branch -M main
git remote add origin https://github.com/USER/REPO.git
git push -u origin main
```

If this folder is already a repository, commit the files and push them to `main` instead of running `git init`.

### 3. Turn on Pages from the branch

1. Open the repository on GitHub.
2. Select **Settings**. If you do not see it, open the repository menu and choose **Settings**.
3. In the sidebar, under **Code, planning, and automation**, select **Pages**.
4. Under **Build and deployment**, set **Source** to **Deploy from a branch**.
5. Set the branch to `main` and the folder to **/ (root)**.
6. Select **Save**.

`index.html` has to stay at the root of `main`. Pages looks for it in the folder you selected, not in a subfolder.

The `.nojekyll` file tells Pages to publish the files as they are, without running Jekyll.

### 4. Open the site

GitHub shows the site URL at the top of the Pages settings once the first deploy finishes. That usually takes a minute or two.

- A project site (any other repository name) is published at `https://USER.github.io/REPO/`.
- A user site is published at `https://USER.github.io/`. The repository must be named exactly `USER.github.io`, with your username.

Later pushes to `main` update the same URL.

### Optional: deploy with GitHub Actions

Use this only if you want Actions to publish the site. In **Settings → Pages**, set **Source** to **GitHub Actions** before you add the workflow. Leave this file out if you are publishing from the branch, as in the steps above.

Create `.github/workflows/pages.yml`:

```yaml
name: Deploy static content to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v6
      - name: Setup Pages
        uses: actions/configure-pages@v5
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v4
        with:
          path: .
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Push that file to `main`. The workflow run prints the site URL when the deploy job finishes.

### Custom domain

In **Settings → Pages**, enter a domain under **Custom domain**. GitHub's guide covers the DNS records: [Configuring a custom domain for your GitHub Pages site](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).
