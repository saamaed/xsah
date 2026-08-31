---

title: "Building XSAH with Hugo and Blowfish"
date: 2026-08-30
description: "Setting up, customizing and deploying the XSAH website with Hugo, Blowfish and GitHub Pages."
tags:
  - hugo
  - blowfish
  - git
  - github
  - github-pages
categories:
  - notes

---

## Overview

XSAH is my personal website for documenting my technical journey, projects, experiments, and notes.

The site is built with **Hugo SSG** and the **Blowfish** theme, with **Git** used for version control and **GitHub** used for repository hosting. Deployment is handled through **GitHub Actions** and **GitHub Pages**.

This note documents the initial setup, customization, and deployment of the website.

## Installing Hugo

The first step was installing the **[Extended version of Hugo](https://gohugo.io/installation/)** was chosen because it provides more features and capabilities than the standard version, including additional asset processing features required by themes such as Blowfish.

**Hugo Extended** was installed and verified with:

```console
$ hugo version
```

The installed version was:

```text
hugo v0.165.0 ... extended linux/amd64
```

A new Hugo site was initialized, followed by moving into its directory:

```console
$ hugo new site xsah
$ cd xsah
```

## Initializing Git

[Git](https://git-scm.com/) was used for version control, making it possible to track changes to the website, maintain its history, and safely manage updates over time.

Git was initialized locally, and the default branch was renamed to **main**:

```console
$ git init
$ git branch -M main
```

The GitHub repository was added as the remote:

```console
$ git remote add origin git@github.com:<username>/<repository>.git
```

## Adding the Blowfish Theme

[Blowfish](https://blowfish.page/docs) was added to the project as a [Git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules). Hugo also provides its own [module system](https://gohugo.io/hugo-modules/), which can be used to manage themes and other dependencies.

For this project, however, Git submodules were preferred to keep the theme as a separate Git-managed dependency:

```console
$ git submodule add https://github.com/nunocoracao/blowfish.git themes/blowfish
```

Using a submodule keeps the theme repository separate from the website repository while allowing the project to track a specific theme version.

The theme can be updated when needed with:

```console
$ git submodule update --remote --merge
```


## Shallow Clone of the Blowfish Repository

For obtaining the theme source without downloading its complete Git history, a shallow clone can be used:

```console
$ git clone --depth 1 https://github.com/nunocoracao/blowfish.git
```

This retrieves the current theme with only the latest revision, reducing unnecessary repository history.

The required example configuration and content were then used as the starting point for the XSAH website.

## Cleaning the Default Configuration

The [Blowfish example](https://blowfish.page/examples/) site contains a large amount of demonstration content and configuration intended to showcase the theme's features.

For XSAH, unnecessary example content and multilingual configuration were removed to create a clean and maintainable starting point.

The demo content under `content/` was removed while preserving the main `_index.md` file:

```console
$ find content -mindepth 1 -type f ! -name "_index.md" -delete
```

The example site's content and static files were removed:

```console
$ rm -rf themes/blowfish/exampleSite/content
$ rm -rf themes/blowfish/exampleSite/static
```

Only English is currently used, so the additional language configuration files were removed:

```console
$ rm config/_default/languages.de.toml
$ rm config/_default/languages.es.toml
$ rm config/_default/languages.fr.toml
$ rm config/_default/languages.it.toml
$ rm config/_default/languages.ja.toml
$ rm config/_default/languages.pt-br.toml
$ rm config/_default/languages.pt-pt.toml
$ rm config/_default/languages.zh-cn.toml
```

The corresponding unused menu configurations were removed as well:

```console
$ rm config/_default/menus.de.toml
$ rm config/_default/menus.es.toml
$ rm config/_default/menus.fr.toml
$ rm config/_default/menus.it.toml
$ rm config/_default/menus.ja.toml
$ rm config/_default/menus.pt-br.toml
$ rm config/_default/menus.pt-pt.toml
$ rm config/_default/menus.zh-cn.toml
```

The example site's authors, examples, users, and demo tag content were no longer required:

```console
$ rm -rf content/authors
$ rm -rf content/examples
$ rm -rf content/users
$ rm -rf content/tags/advanced
```

Unused multilingual taxonomy files were also removed:

```console
$ rm content/tags/_index.de.md
$ rm content/tags/_index.es.md
$ rm content/tags/_index.fr.md
$ rm content/tags/_index.it.md
$ rm content/tags/_index.ja.md
$ rm content/tags/_index.pt-br.md
$ rm content/tags/_index.pt-pt.md
$ rm content/tags/_index.zh-cn.md
```

After each cleanup stage, the site was rebuilt with Hugo to verify that the remaining configuration and content were still valid:

```console
$ hugo
```

The project was then reduced to a simpler structure focused on the actual website:

```text
.
├── archetypes/
├── assets/
├── config/
├── content/
├── static/
├── themes/
└── hugo.toml
```

The purpose was to remove the unnecessary parts of the Blowfish demo while keeping the core structure required by the XSAH website.

## Adding the XSAH Branding

After cleaning the demo content, the remaining Blowfish configuration was adapted to the XSAH project.

The site identity is defined in:

```console
$ nano config/_default/languages.en.toml
```

The original Blowfish title and description were replaced with the XSAH branding:

```toml
title = "Xsah"
description = "....."
```

The theme's logo and author information were also adapted to the XSAH identity.

Project-specific theme settings remain in:

```console
$ config/_default/params.toml
```

This keeps the customisation separate from the Blowfish theme itself.

## Customizing the Homepage

The homepage was configured to use Blowfish's `profile` layout:

```toml
[homepage]
  layout = "profile"
  showRecent = true
  showRecentItems = 12
  showMoreLink = true
```

The homepage content and introduction were then defined in:

```console
$ nano content/_index.md
```

The page title and description were set to reflect the purpose of XSAH:

```yaml
title: "Learn. Build. Repeat."
description: "....."
```

A custom section was added between the profile and recent articles, containing three feature cards for **Linux**, **DevOps**, and **Cloud**.

Recent articles were enabled through Blowfish's built-in [Homepage Layout](https://blowfish.page/docs/homepage-layout/) configuration. The `mainSections` parameter was set to `notes`, making the `content/notes/` section the source for the homepage's Recent Articles list.

The existing Blowfish layout and shortcode mechanisms were reused instead of modifying the theme's core templates.


## Creating the First Post

A first Markdown post was created to document the website setup:

```console
$ hugo new notes/building-xsah.md
```

Hugo uses front matter at the beginning of the Markdown file to define metadata such as the title, date, description, tags, and category.

For example:

```yaml
---
title: "Building XSAH with Hugo and Blowfish"
date: 2026-08-30
description: "....."
tags:
  - hugo
  - blowfish
  - git
  - github
  - github-pages
categories:
  - notes
---
```

The website can be previewed locally with:

```console
$ hugo server
```

The local development server is then available at browser:

```text
http://localhost:1313/
```

## GitHub Actions Deployment

Deployment was automated using [GitHub Actions](https://github.com/features/actions) and [GitHub Pages](https://pages.github.com/).

A workflow was added under:

```text
.github/
└── workflows/
    └── hugo.yaml
```

The workflow checks out the repository, installs the required Hugo version, builds the site, and deploys the generated files to GitHub Pages. It is based on the default [Hugo workflow](https://github.com/actions/starter-workflows/blob/main/pages/hugo.yml) provided by GitHub:


```text
Local changes
      ↓
git add
      ↓
git commit
      ↓
git push
      ↓
GitHub Actions
      ↓
Hugo build
      ↓
GitHub Pages
      ↓
XSAH website
```

After committing and pushing the workflow, GitHub automatically builds and deploys the site to GitHub Pages whenever changes are pushed to the main branch.


## Committing and Pushing the Changes

The repository was connected to [GitHub using SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh), allowing changes to be pushed without entering GitHub credentials for every operation.

After completing the configuration, customization, branding, and initial content, the changes were committed and pushed to the `main` branch:

```console
$ git status
$ git add .
$ git commit -m "Set up and customize XSAH website"
```

The changes were then pushed to GitHub:

```console
$ git push
```

GitHub Actions automatically handled the build and deployment process.

## Result

The original Blowfish demo site was transformed into a clean, minimal XSAH website with a structure focused on Linux, DevOps, Cloud, and hands-on projects.

The homepage now presents the XSAH profile, topic cards, and recent notes, while the navigation provides access to the main sections of the site.

The project is version-controlled with Git, hosted on GitHub, and automatically built and deployed to GitHub Pages through GitHub Actions.

This provides a clean foundation for the next stage: documenting the learning journey, adding hands-on projects, and gradually building the site into a technical knowledge base.

If you want to customize template  further, see the [Advanced Customisation](https://blowfish.page/docs/advanced-customisation/) guide.


---

**Stack:** Hugo · Blowfish · Git · GitHub · GitHub Actions · GitHub Pages
