# Isabella Robot Wiki

A comprehensive Zensical wiki for the Isabella Robot project documentation.

## Overview

This wiki is built with:
- **[Zensical](https://zensical.org/)** - Modern static site generator (Material Design theme built-in)
- **[Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)** - Underlying theme (included with Zensical)
- **GitHub Pages** - Free hosting via GitHub Actions CI/CD

Zensical is a modern replacement for MkDocs with better performance, faster builds, and enhanced features.

## Project Structure

```
Isabella_Robot_Doc/
├── .github/
│   └── workflows/
│       └── docs.yml              # GitHub Actions deployment workflow
├── docs/                          # Documentation source files
│   ├── index.md                  # Homepage
│   ├── getting-started/          # Getting Started section
│   ├── hardware/                 # Hardware documentation
│   ├── software/                 # Software architecture
│   ├── architecture/             # System design
│   ├── networking/               # Network & Docker
│   └── guides/                   # Guides and tutorials
├── site/                         # Generated static site (build output)
├── zensical.toml                 # Zensical configuration
└── requirements.txt              # Python dependencies
```

## Quick Start

### Local Development

1. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

2. **Preview documentation locally**
   ```bash
   zensical serve
   ```
   Open http://localhost:8000 in your browser

3. **Build static site**
   ```bash
   zensical build
   ```
   Output generated in `site/` directory

## Documentation Structure

### Adding New Pages

1. Create a `.md` file in the appropriate `docs/` subdirectory
2. Update the `nav` section in `zensical.toml` to add the page to navigation
3. Rebuild or refresh to see changes

Example:
```toml
[[nav]]
name = "Hardware"
children = [
    { "New Device" = "hardware/new-device.md" }
]
```

### Markdown Features

The wiki supports:
- **Code highlighting** with syntax coloring
- **Admonitions** (notes, warnings, tips)
- **Tables and lists**
- **LaTeX math** (via extensions)
- **Emoji support**
- **Tabbed content**
- **Mermaid diagrams**

Example admonition:
```markdown
!!! note
    This is important information
```

## Configuration Files

### zensical.toml

Main configuration file (replaces `mkdocs.yml`):
- Site metadata (name, description, URL)
- Theme settings (Material Design built-in)
- Plugin configuration
- Navigation structure
- Markdown extensions

### requirements.txt

Python package dependencies for building the docs.

## Customization

### Custom Styling

Add custom CSS in `docs/stylesheets/extra.css`:

```yaml
# mkdocs.yml
extra_css:
  - stylesheets/extra.css
```

### Custom JavaScript

Add custom JS in `docs/javascripts/extra.js`:

```yaml
# mkdocs.yml
extra_javascript:
  - javascripts/extra.js
```

### Theme Colors

Edit the `palette` section in `mkdocs.yml`:

```yaml
palette:
  - scheme: default
    primary: blue
    accent: amber
  - scheme: slate
    primary: blue
    accent: amber
```

## Deployment

### GitHub Pages (Automatic)

The included GitHub Actions workflow automatically:
1. Builds the documentation when you push to `main`/`master`
2. Deploys to GitHub Pages
3. Makes the site available at `https://nm-tafe.github.io/Isabella_Robot_Doc/`

**To enable:**
1. Go to repository Settings → Pages
2. Set source to "Deploy from a branch"
3. Select `gh-pages` branch

### Manual Deployment

```bash
mkdocs gh-deploy
```

This builds and pushes to the `gh-pages` branch automatically.

## Commands

```bash
# Serve locally with hot reload
zensical serve

# Build static site
zensical build

# Deploy to GitHub Pages
git add -A && git commit -m "Update docs" && git push origin main
# (GitHub Actions handles deployment automatically)

# Clean build artifacts
rm -rf site/ && zensical build
```

## Documentation Content

Current sections:

- **Getting Started** - Overview, quick start, testing
- **Hardware** - Depth camera, LiDAR, docking station, Respeaker
- **Software** - ROS nodes, LLM server, localizer, perception
- **Architecture** - System design, intents, persistence
- **Networking** - Ports, Docker configuration
- **Guides** - Specific how-to guides

## Contributing

To add documentation:

1. Create or edit `.md` files in `docs/`
2. Update `mkdocs.yml` navigation as needed
3. Test locally with `mkdocs serve`
4. Commit and push changes
5. GitHub Actions will automatically build and deploy

## Troubleshooting

### Build fails with missing files

Ensure file paths in `mkdocs.yml` match actual file names (case-sensitive).

### Assets/images not showing

Place media files in `docs/media/` and reference them:
```markdown
![Alt text](../media/image.png)
```

### Port 8000 already in use

Change port: `mkdocs serve --dev-addr 127.0.0.1:8001`

## Resources

- [Zensical Documentation](https://zensical.org/)
- [Material Design Theme](https://squidfunk.github.io/mkdocs-material/)
- [Markdown Guide](https://www.markdownguide.org/)

## License

See the main repository LICENSE file for details.
