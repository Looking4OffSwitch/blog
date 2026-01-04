# Looking4OffSwitch's Blog

A personal blog built with Jekyll and the [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/) theme, hosted on GitHub Pages.

## Prerequisites

### Install Ruby

**macOS:**
```bash
brew install ruby
```

**Ubuntu/Debian:**
```bash
sudo apt-get install ruby-full build-essential
```

**Windows:**
Download and install from [RubyInstaller](https://rubyinstaller.org/)

### Install Bundler

```bash
gem install bundler
```

## Local Development

### Install dependencies

```bash
bundle install
```

### Run the local server

```bash
./run_local.sh
```

Or directly:
```bash
bundle exec jekyll serve
```

Open `http://localhost:4000/blog` in your browser.

The server watches for changes and auto-rebuilds. Press `Ctrl+C` to stop.

### Build without serving

```bash
bundle exec jekyll build
```

Output goes to the `_site/` directory.

## Creating New Posts

Posts live in the `_posts/` directory and must follow this naming convention:

```
YYYY-MM-DD-title-of-post.md
```

### Post template

```markdown
---
layout: single
title: "Your Post Title"
date: 2026-01-04
categories: category1 category2
tags: tag1 tag2
---

Your content here. Write in Markdown.
```

### Front matter options

| Field | Required | Description |
|-------|----------|-------------|
| `layout` | Yes | Use `single` for blog posts |
| `title` | Yes | The post title (displayed as heading) |
| `date` | Yes | Publication date (YYYY-MM-DD) |
| `categories` | No | Space-separated categories |
| `tags` | No | Space-separated tags |
| `toc` | No | Set `true` to show table of contents |
| `toc_sticky` | No | Set `true` to make TOC sticky on scroll |
| `excerpt` | No | Custom excerpt for post previews |

## Markdown Formatting

### Headers

```markdown
# H1
## H2
### H3
```

### Text styling

```markdown
**bold**
*italic*
~~strikethrough~~
`inline code`
```

### Links and images

```markdown
[Link text](https://example.com)
![Alt text](/path/to/image.jpg)
```

### Code blocks

````markdown
```python
def hello():
    print("Hello, world!")
```
````

### Lists

```markdown
- Unordered item
- Another item

1. Ordered item
2. Another item
```

### Blockquotes

```markdown
> This is a blockquote
```

### Tables

```markdown
| Header 1 | Header 2 |
|----------|----------|
| Cell 1   | Cell 2   |
```

### Notices/Callouts

```markdown
**Note:** This is a notice.
{: .notice}

**Warning:** This is a warning.
{: .notice--warning}

**Success:** This worked!
{: .notice--success}
```

## Changing the Theme

### Theme skins

Minimal Mistakes includes several color skins. Change in `_config.yml`:

```yaml
minimal_mistakes_skin: "dark"  # "default", "air", "aqua", "contrast", "dark", "dirt", "neon", "mint", "plum", "sunrise"
```

### Customizing styles

Create `assets/css/main.scss` to add custom styles:

```scss
---
---

@import "minimal-mistakes/skins/{{ site.minimal_mistakes_skin | default: 'default' }}";
@import "minimal-mistakes";

// Your custom styles here
```

### Using a different theme

1. Find a theme at [rubygems.org](https://rubygems.org/search?query=jekyll-theme) or [jekyllthemes.io](https://jekyllthemes.io/)

2. Update `Gemfile`:
   ```ruby
   gem "jekyll-theme-name"
   ```

3. Update `_config.yml`:
   ```yaml
   theme: jekyll-theme-name
   ```

4. Run `bundle install` and restart the server

## Configuration

Edit `_config.yml` to customize:

```yaml
title: Your Blog Title
subtitle: Your tagline
description: A short description

author:
  name: Your Name
  bio: "A short bio"
  location: "City, Country"
  links:
    - label: "Email"
      icon: "fas fa-fw fa-envelope-square"
      url: "mailto:you@example.com"
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      url: "https://github.com/username"
```

**Note:** Changes to `_config.yml` require a server restart.

## Deployment

Push to the `main` branch and GitHub Pages will automatically build and deploy.

The site will be available at: `https://Looking4OffSwitch.github.io/blog`

## Resources

- [Minimal Mistakes Documentation](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/)
- [Jekyll Documentation](https://jekyllrb.com/docs/)
