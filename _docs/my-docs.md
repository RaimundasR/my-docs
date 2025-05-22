---
title: My docs and findings
layout: default
---
## My Jekyll automation

### 1. Sukurkite GitHub repozitoriją pavadinimu `my-docs`

1. Nueikite į GitHub ir sukurkite naują repozitoriją: `my-docs`
2. Tiesiai GitHub'e pridėkite failą `readme.md` su turiniu:

```md
## My Jekyll automation
```

---

### 2. Nusiklonuokite repozitoriją lokaliai ir susikurkite šiuos failus ir struktūra:

```bash
.
├── _config_preview.yml
├── _config_release.yml
├── _config_theme.yml
├── _config.yml
├── _docs
│   └── my-docs.md
├── index.md
└── readme.md
```

#### `my-docs.md`

```md
---
title: My docs
layout: default
---
# Mano dokumentacija
```

#### `index.md`

```md
---
layout: default
title: Welcome
---

- [My docs]({{ site.baseurl }}/docs/my-docs/)
```

#### `_config.yml`

```yaml
title: My Docs Site
theme: minima
markdown: kramdown

collections:
  docs:
    output: true
    permalink: /:collection/:name/

defaults:
  - scope:
      path: ""
      type: docs
    values:
      layout: default
```

#### `_config_theme.yml`

```yaml
theme: minima
```

#### `_config_preview.yml`

```yaml
baseurl: "/my-docs/preview"
```

#### `_config_release.yml`

```yaml
baseurl: "/my-docs/doc-0.0.1"
```

---

### 3. Sukurkite workflow failus:

#### `.github/workflows/deploy-preview.yml`

```yaml
name: Deploy Preview (develop)

on:
  push:
    branches: [develop]

permissions:
  contents: write

jobs:
  preview:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.1'
          bundler-cache: true

      - name: Install Jekyll + minima and build
        run: |
          gem install jekyll:4.3.2 bundler:2.4.22 minima:2.5.1 jekyll-remote-theme:0.4.3 jekyll-sass-converter:3.0.0
          echo "theme: minima" > _config_theme.yml
          jekyll build --config _config.yml,_config_theme.yml,_config_preview.yml -d _site
          touch _site/.nojekyll


      - name: Deploy to gh-pages/preview/
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_branch: gh-pages
          publish_dir: ./_site
          destination_dir: preview

```

#### `.github/workflows/release-docs.yml`

```yaml
name: Manual Doc Release

on:
  workflow_dispatch:
    inputs:
      version:
        description: 'Release folder (e.g., doc-0.0.1)'
        required: true

permissions:
  contents: write

jobs:
  build-and-release:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout source
        uses: actions/checkout@v4

      - name: Set up Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.1'
          bundler-cache: true

      - name: Install Jekyll and minima theme
        run: |
          gem install jekyll bundler minima
          echo "theme: minima" > _config_theme.yml
          cat <<EOF > _config_release.yml
          baseurl: "/my-docs/${{ github.event.inputs.version }}"
          EOF
          jekyll build --config _config.yml,_config_theme.yml,_config_release.yml -d _site
          touch _site/.nojekyll

      - name: Deploy to /${{ github.event.inputs.version }}/
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_branch: gh-pages
          publish_dir: ./_site
          destination_dir: ${{ github.event.inputs.version }}

      - name: Generate root redirect to latest version
        run: |
          mkdir redirect
          echo '<meta http-equiv="refresh" content="0; url=./${{ github.event.inputs.version }}/">' > redirect/index.html

      - name: Deploy redirect index.html to root (preserve /preview/)
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_branch: gh-pages
          publish_dir: ./redirect
          destination_dir: .
          keep_files: true

      - name: Publish GitHub Release
        uses: softprops/action-gh-release@v1
        with:
          tag_name: ${{ github.event.inputs.version }}
          name: Docs ${{ github.event.inputs.version }}
          body: |
            🗂️ View the documentation at:
            👉 https://<your_suer_name>.github.io/my-docs/${{ github.event.inputs.version }}/
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

```

---

### 4. Sukurkite ir sukonfigūruokite GitHub šakas

```bash
git checkout -b master
git push -u origin master
```

Tada GitHub nustatymuose:

* Nustatykite `master` kaip default branch (vietoje `main`)

Tada:

```bash
git checkout -b develop
git push -u origin develop
```

* Patikrinkite, ar veikia `Deploy Preview` workflow.

---

### 5. Atlikite PR ir leidimą

1. Atidarykite PR iš `develop` į `master` ir atlikite merge
2. Paleiskite `Manual Doc Release` workflow su release numeriu: `doc-0.0.0`
3. Nustatykite GitHub Pages: `gh-pages` branch ir `/` kaip root directory

---

### 6. Testuokite publikavimą

1. `develop` branch'e redaguokite `my-docs.md`
2. Padarykite `commit` ir `push`
3. Atlikite PR iš `develop` į `master`
4. Vėl paleiskite `Manual Doc Release` su release numeriu `doc-0.0.1`

---

### 7. Peržiūrėkite puslapius:

* Peržiūrai (preview):

  ```
  ```

[https://your\_username.github.io/my-docs/preview/](https://your_username.github.io/my-docs/preview/)

```

- Versijai (release):
```

[https://your\_username.github.io/my-docs/doc-0.0.1/](https://your_username.github.io/my-docs/doc-0.0.1/)

```

Sėkmės automatizuojant dokumentaciją! ✨

```

