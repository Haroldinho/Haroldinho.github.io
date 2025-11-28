# Harold Nikoue's Personal Website

This repository contains the source code for Harold Nikoue's personal website, hosted on GitHub Pages. The website serves as a repository of what I'm learning and the projects I'm working on.

**Live Site:** [https://haroldinho.github.io](https://haroldinho.github.io)

The website is built using [Jekyll](https://jekyllrb.com/) and the [Chirpy](https://chirpy.cotes.page/) theme.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Local Development](#local-development)
- [GitHub Actions Deployment](#github-actions-deployment)
- [Adding New Posts](#adding-new-posts)
- [Project Structure](#project-structure)
- [License](#license)

## Prerequisites

Before you begin, ensure you have the following installed:

- **Ruby** (version 3.3 or compatible)
  - Check your Ruby version: `ruby --version`
  - Install Ruby using [rbenv](https://github.com/rbenv/rbenv) or [rvm](https://rvm.io/)
- **Bundler** (Ruby gem manager)
  - Install: `gem install bundler`
- **Node.js** (version 18 or higher)
  - Check your Node version: `node --version`
  - Download from [nodejs.org](https://nodejs.org/)
- **npm** (comes with Node.js)
  - Check your npm version: `npm --version`

## Local Development

Follow these steps to run the website locally on your machine:

### 1. Clone the Repository

```bash
git clone https://github.com/Haroldinho/Haroldinho.github.io.git
cd Haroldinho.github.io
```

### 2. Install Ruby Dependencies

```bash
bundle install
```

This will install all the Jekyll plugins and dependencies specified in the `Gemfile`.

### 3. Install Node.js Dependencies

```bash
npm install
```

This will install the frontend dependencies (Bootstrap, Popper.js, etc.) and build tools.

### 4. Build Assets

Before running the site, you need to build the CSS and JavaScript assets:

```bash
npm run build
```

This command will:

- Build and minify CSS using PurgeCSS
- Bundle and minify JavaScript using Rollup

### 5. Run the Jekyll Server

Start the local development server:

```bash
bundle exec jekyll serve
```

Or with live reload enabled:

```bash
bundle exec jekyll serve --livereload
```

The site will be available at `http://localhost:4000` (or the port shown in the terminal).

### 6. Watch for Changes (Optional)

If you're making changes to JavaScript or SCSS files, you can run the watch command in a separate terminal:

```bash
npm run watch:js
```

This will automatically rebuild JavaScript files when you make changes.

### Troubleshooting Local Development

- **Port already in use**: If port 4000 is busy, use `bundle exec jekyll serve --port 4001`
- **Dependencies issues**: Try `bundle update` and `npm update`
- **Build errors**: Make sure you've run `npm run build` before starting the server

## GitHub Actions Deployment

The website is automatically deployed to GitHub Pages using GitHub Actions whenever you push changes to the `main` branch.

### How It Works

The deployment workflow (`.github/workflows/pages-deploy.yml`) automatically:

1. **Triggers** on:
   - Push to `main` or `master` branch
   - Manual trigger from the Actions tab (workflow_dispatch)
   - Ignores changes to `.gitignore`, `README.md`, and `LICENSE`

2. **Build Process**:
   - Checks out the repository
   - Sets up Ruby 3.3 and installs dependencies
   - Builds the Jekyll site in production mode
   - Tests the site using HTMLProofer (checks for broken links)
   - Uploads the built site as an artifact

3. **Deployment**:
   - Deploys the built site to GitHub Pages
   - The site is available at `https://haroldinho.github.io`

### Manual Deployment

You can manually trigger a deployment:

1. Go to the **Actions** tab in your GitHub repository
2. Select **Build and Deploy** workflow
3. Click **Run workflow**
4. Select the branch (usually `main`) and click **Run workflow**

### Viewing Deployment Status

- Check the **Actions** tab to see the status of recent deployments
- Green checkmark = successful deployment
- Red X = deployment failed (check logs for errors)

### Common Deployment Issues

- **Build failures**: Check the Actions logs for specific error messages
- **Missing dependencies**: Ensure `Gemfile` and `package.json` are up to date
- **HTML validation errors**: HTMLProofer may catch broken links or invalid HTML

## Adding New Posts

To add a new blog post:

1. Create a new Markdown file in the `_posts/` directory
2. Name the file using the format: `YYYY-MM-DD-post-title.md`
   - Example: `2025-01-15-my-new-post.md`
3. Add front matter at the top of the file:

```yaml
---
title: Your Post Title
date: 2025-01-15
categories: [Category Name]
tags: [tag1, tag2]
---
```

1. Write your content in Markdown below the front matter
2. Commit and push to trigger automatic deployment

### Post Images

Place images in `assets/img/` or `_posts/figures/` and reference them in your posts:

```markdown
![Image description](/assets/img/your-image.png)
```

## Project Structure

```text
.
├── _config.yml          # Jekyll configuration
├── _posts/              # Blog posts (Markdown files)
├── _tabs/               # Navigation tabs (About, Archives, etc.)
├── _sass/               # SCSS stylesheets
├── _javascript/         # JavaScript source files
├── _includes/           # Jekyll includes (reusable components)
├── _layouts/            # Page layouts
├── _data/               # Data files (authors, locales, etc.)
├── assets/              # Static assets (images, CSS, JS)
├── .github/
│   └── workflows/      # GitHub Actions workflows
├── Gemfile              # Ruby dependencies
├── package.json         # Node.js dependencies
└── README.md           # This file
```

## License

This project is published under [MIT License][license].

## Resources

- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [Chirpy Theme Documentation](https://chirpy.cotes.page/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)

[license]: https://github.com/cotes2020/jekyll-theme-chirpy/blob/master/LICENSE
