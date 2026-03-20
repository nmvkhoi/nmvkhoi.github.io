# nmvkhoi.github.io

This is my personal blog built with Hugo static site generator and hosted on GitHub Pages.

## Blog Setup Tutorial

This tutorial will guide you through setting up a similar Hugo-based blog from scratch. The blog features a clean, minimal design with support for posts, categories, tags, and an about page.

### Prerequisites

- Git (for version control and GitHub deployment)
- A GitHub account (for hosting)
- Basic knowledge of Markdown

### Step 1: Install Hugo

Hugo is a fast static site generator written in Go. Install it on your system:

#### On Linux/macOS

```bash
# Using snap (recommended for Linux)
sudo snap install hugo

# Or download from GitHub releases
# Visit https://github.com/gohugoio/hugo/releases and download the latest extended version for your OS
```

#### On Windows

Download the Windows executable from [Hugo releases](https://github.com/gohugoio/hugo/releases).

#### Verify installation

```bash
hugo version
```

### Step 2: Create a New Hugo Site

1. Create a new directory for your blog source (separate from the built site):

```bash
mkdir my-blog-source
cd my-blog-source
```

2. Initialize a new Hugo site

```bash
hugo new site .
```

This creates the basic Hugo directory structure:

```bash
my-blog-source/
├── archetypes/
├── assets/
├── content/
├── data/
├── layouts/
├── static/
├── themes/
└── config.toml
```

### Step 3: Choose and Install a Theme

For a clean, minimal theme similar to this blog, you can use a simple theme or create a custom one. Here are some options:

#### Option A: Use a Minimal Theme

```bash
# Clone a minimal theme (example: PaperMod)
git clone https://github.com/adityatelange/hugo-PaperMod themes/PaperMod --depth=1

# Or use the built-in theme system
# For this blog's style, you might need to customize
```

#### Option B: Create a Custom Minimal Theme

Create the following files in your `layouts/` directory:

**layouts/_default/baseof.html:**

```html
<!DOCTYPE html>
<html lang="{{ .Site.LanguageCode }}">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>{{ .Title }}</title>
    <link rel="stylesheet" href="/css/main.css" />
</head>
<body>
    <main class="page-content">
        <div class="w">
            <header>
                <h1>{{ .Site.Title }}</h1>
            </header>
            {{ block "main" . }}{{ end }}
        </div>
    </main>
</body>
</html>
```

**layouts/_default/list.html:**

```html
{{ define "main" }}
<ul>
{{ range .Pages }}
    <li>
        <span>{{ .Date.Format "2006-01-02" }}</span>
        <a href="{{ .Permalink }}">{{ .Title }}</a>
    </li>
{{ end }}
</ul>
{{ end }}
```

**layouts/_default/single.html:**

```html
{{ define "main" }}
<a href="/">..</a>
<article>
    <p class="post-meta">
        <time datetime="{{ .Date }}">{{ .Date.Format "2006-01-02" }}</time>
    </p>
    <h1>{{ .Title }}</h1>
    {{ .Content }}
</article>
{{ end }}
```

### Step 4: Configure Your Site

Edit `config.toml`:

```toml
baseURL = "https://yourusername.github.io/"
languageCode = "en-us"
title = "Your Blog Name"
theme = "your-theme-name"  # if using a theme

[params]
    author = "Your Name"
    description = "Your blog description"
```

### Step 5: Create Content

#### Create an About Page

```bash
hugo new about.md
```

Edit `content/about.md`:

```markdown
---
title: "About"
date: 2023-08-30T00:00:00+07:00
---

Write your about content here.
```

#### Create Blog Posts

```bash
hugo new posts/first-post.md
```

Edit `content/posts/first-post.md`:

```markdown
---
title: "My First Post"
date: 2023-08-30T00:00:00+07:00
draft: false
---

Your post content here. You can use Markdown formatting.

## Heading

- List item
- Another item

[Link](https://example.com)
```

### Step 6: Add Custom CSS and JavaScript

Create `static/css/custom.css` for custom styles:

```css
/* Your custom styles */
body {
    font-family: Arial, sans-serif;
}

.w {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
}
```

Create `static/js/custom.js` for custom scripts if needed.

### Step 7: Build the Site

Generate the static files:

```bash
hugo
```

This creates a `public/` directory with your built site.

### Step 8: Deploy to GitHub Pages

#### Option A: Deploy Built Site Only (like this repo)

1. Create a new GitHub repository named `yourusername.github.io`
2. Copy the contents of `public/` to the repository root
3. Push to GitHub

#### Option B: Deploy Source with GitHub Actions

1. Create two repositories:
   - `yourusername.github.io` for the built site
   - `blog-source` for your Hugo source

2. In your source repository, create `.github/workflows/deploy.yml`:

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: true
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

### Step 9: Local Development

Run Hugo's development server:

```bash
hugo server -D
```

Visit `http://localhost:1313` to preview your site. The `-D` flag includes draft posts.

### Step 10: Writing Posts

- Use Markdown for content
- Front matter (YAML/TOML) for metadata
- Images go in `static/img/`
- Categories and tags are supported automatically

### Customization Tips

- Modify layouts in `layouts/` for custom design
- Add custom CSS in `static/css/`
- Use Hugo's built-in functions for dynamic content
- Check Hugo documentation for advanced features

### Useful Commands

```bash
# Create new post
hugo new posts/my-post.md

# Build site
hugo

# Serve locally
hugo server

# Clean public directory
rm -rf public/
```

### Resources

- [Hugo Documentation](https://gohugo.io/documentation/)
- [Hugo Themes](https://themes.gohugo.io/)
- [Markdown Guide](https://www.markdownguide.org/)

This setup provides a fast, secure, and maintainable blog that can be hosted for free on GitHub Pages.
