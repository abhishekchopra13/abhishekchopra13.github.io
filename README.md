# abhishekchopra13.github.io

My personal website for self-documentation, blogs, and notes. Built using Jekyll and the Minima theme, with LaTeX support configured for posts.

## Getting Started / Local Development

Depending on your operating system, follow the relevant guide below to spin up a local development server.

---

### Method 1: Using NixOS / Nix

This repository contains a `shell.nix` file defining the development environment.

1. **Enter the Nix Shell**:
   ```bash
   nix-shell
   ```
   *This loads Ruby, git, sqlite, gnumake, and necessary C libraries.*

2. **Install Ruby Dependencies**:
   ```bash
   bundle install
   ```

3. **Run the Jekyll Development Server**:
   ```bash
   RUBYOPT="-W:no-deprecated" bundle exec jekyll serve
   ```
   Alternatively, you can run it in a single command from your host shell:
   ```bash
   nix-shell --run "RUBYOPT=\"-W:no-deprecated\" bundle exec jekyll serve"
   ```

4. **Access the Site**:
   Open your browser and navigate to `http://localhost:4000/`.

---

### Method 2: Using macOS (Standard Ruby & Bundler)

If you are running macOS and do not wish to use Nix, you can run Jekyll directly using your local Ruby installation.

1. **Install Bundler** (if not already installed):
   ```bash
   gem install bundler
   ```

2. **Install Dependencies**:
   ```bash
   bundle install
   ```

3. **Run the Jekyll Development Server**:
   ```bash
   RUBYOPT="-W:no-deprecated" bundle exec jekyll serve
   ```

4. **Access the Site**:
   Open your browser and navigate to `http://localhost:4000/`.

---

## Writing New Posts

1. Create a new markdown file in the `_posts/` directory. The filename must follow the format `YYYY-MM-DD-post-title.md` (e.g., `_posts/2026-06-16-fx-pnl-computation.md`).
2. Include the standard YAML front matter at the top of the file:
   ```yaml
   ---
   layout: post
   title: "Your Post Title"
   date: YYYY-MM-DD HH:MM:SS +0000
   categories: your-category
   ---
   ```
3. Use standard Markdown for content. To render math/LaTeX equations, write:
   - **Inline Math**: `$$a^2 + b^2 = c^2$$` (wrapped in double-dollar signs inline)
   - **Display/Block Math**: Put `$$` on separate lines around the math block.
4. Manually link to the new post in `blogs.markdown`.
