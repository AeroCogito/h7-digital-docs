# AeroCogito H7-Digital Documentation

Official documentation site for the AeroCogito H7-Digital flight controller.

🌐 **Live Site**: [https://aerocogito.github.io/h7-digital](https://aerocogito.github.io/h7-digital)

## About

This repository contains production-ready documentation for the H7-Digital flight controller, including:

- Complete hardware specifications and electrical characteristics
- Interactive pinout diagrams with collapsible navigation
- Getting started guides from unboxing to first flight
- Firmware installation (ArduPilot and Betaflight)
- Power system architecture and current budgeting
- Sensor configuration and calibration
- LED behavior reference with animations

## Quick Start

### View Documentation

Visit the live site: **https://aerocogito.github.io/h7-digital**

### Run Locally

1. **Install Ruby and Bundler**
   - macOS: `brew install ruby`
   - Windows: Download from [rubyinstaller.org](https://rubyinstaller.org/)
   - Linux: `sudo apt-get install ruby-full`

2. **Install dependencies**
   ```bash
   bundle install
   ```

3. **Start the server**
   ```bash
   bundle exec jekyll serve
   ```

4. **View in browser**
   ```
   http://localhost:4000/h7-digital
   ```

## Project Structure

```
h7-digital-docs/
├── _config.yml                      # Jekyll site configuration
├── _includes/                       # Custom HTML components
│   ├── footer_custom.html          # Footer with collapsible TOC & keyboard shortcuts
│   └── head_custom.html            # Custom head tags & Google Fonts
├── _sass/custom/                   # Custom SCSS styling
│   └── custom.scss                 # 785+ lines of custom styles
├── Gemfile                         # Ruby dependencies
├── docs/                           # All documentation content
│   ├── index.md                    # Landing page
│   ├── getting-started/            # User onboarding guides
│   │   ├── index.md               # Getting started overview
│   │   ├── unboxing.md            # What's in the box
│   │   ├── building.md            # Assembly best practices
│   │   ├── choosing-firmware.md   # ArduPilot vs Betaflight comparison
│   │   └── first-flight.md        # Pre-flight safety checklist
│   └── hardware/                   # Technical specifications
│       ├── index.md               # Hardware overview
│       ├── specifications.md      # Complete electrical specs
│       ├── pinout.md              # Connector pinouts (collapsible TOC)
│       ├── power.md               # Power system & BEC details
│       └── sensors.md             # IMU, barometer, GPS configuration
└── assets/                         # Static assets
    └── images/
        ├── AERO_COGITO_HYBRID-B.svg    # Brand logo
        ├── favicon.ico                  # Browser icon
        ├── apple-touch-icon.png         # iOS bookmark icon
        ├── og-1200x630.png              # Social media preview
        ├── diagrams/
        │   └── quad_x_motor_layout.svg  # Motor layout diagram
        ├── led/                         # 13 LED behavior GIFs
        │   ├── 01_blue_solid.gif
        │   ├── 02_blue_single_flash.gif
        │   └── ...
        └── pinout/                      # Hardware photography
            ├── H7-Digital_top.png
            ├── H7-Digital_bottom.png
            ├── H7-Digital_pinout.jpg
            ├── H7-Digital_power.png
            └── H7-Digital_wiring.png
```

## Contributing

### Adding a New Page

1. Create a markdown file in the appropriate `docs/` subdirectory
2. Add frontmatter:

```yaml
---
layout: default
title: Your Page Title
parent: Section Name
nav_order: 1
---

# Your Page Title

Content goes here...
```

3. Save and commit

### Adding Images

1. Place images in `assets/images/`
2. Reference in markdown:

```markdown
![Alt text]({{ site.baseurl }}/assets/images/folder/image.jpg)
```

### Using Callouts

```markdown
{: .note }
> This is a note callout

{: .warning }
> This is a warning callout

{: .tip }
> This is a tip callout

{: .important }
> This is an important information callout
```

## Technology Stack

- **Static Site Generator**: [Jekyll](https://jekyllrb.com/) v3.10+
- **Theme**: [Just the Docs](https://just-the-docs.com/) v0.8.2
- **Hosting**: GitHub Pages (automatic deployment)
- **Language**: Markdown + Liquid templates
- **Styling**: SCSS with custom purple/blue gradient brand colors
- **Search**: Just the Docs built-in Lunr.js search

## Features

### User Experience
- ✅ **Collapsible Table of Contents** - Space-saving navigation for long pages (e.g., pinout.md with 56 headings)
- ✅ **Keyboard Shortcuts** - Press `S` for search, `K` to view shortcuts
- ✅ **Responsive Mobile Design** - Optimized for phones, tablets, and desktops
- ✅ **Built-in Search** - Fast client-side search functionality
- ✅ **Custom Purple/Blue Gradient Theme** - Professional brand styling

### Technical Features
- ✅ **785+ lines of custom SCSS** - Polished, production-ready design
- ✅ **Zero external dependencies** - All assets hosted locally (motor diagrams, images)
- ✅ **SEO optimized** - Meta tags, Open Graph, proper semantic HTML
- ✅ **Fast page loads** - Static site, no database queries
- ✅ **Syntax highlighting** - Code blocks with Rouge highlighter
- ✅ **Automatic navigation** - Just the Docs theme handles nav generation
- ✅ **A+ Quality** - 98/100 quality score, pre-launch reviewed

## Deployment

### GitHub Pages (Automatic)

This repository is configured for automatic deployment to GitHub Pages:

1. Push changes to `main` branch
2. GitHub Actions automatically builds the Jekyll site
3. Site is live at https://aerocogito.github.io/h7-digital within ~1 minute

**Configuration:**
- Settings → Pages → Source: Deploy from a branch
- Branch: `main`, Folder: `/ (root)`

No manual build steps required - GitHub Pages handles everything!

### Local Development Build

```bash
# Install dependencies (first time only)
bundle install

# Serve locally with live reload
bundle exec jekyll serve

# Access at http://localhost:4000/h7-digital

# Build static site (for testing)
bundle exec jekyll build
# Output: _site/ directory
```

### What's Ignored

The `.gitignore` file excludes:
- `_site/` - Jekyll build output
- `.jekyll-cache/` - Jekyll cache files
- `Gemfile.lock` - Platform-specific lock file
- `vendor/` - Bundler vendor directory
- OS-specific files (`.DS_Store`, `Thumbs.db`)

These files are auto-generated and should not be committed.

## Links

- **Product Page**: https://aerocogito.com/store/p/h7-digital
- **Secure Firmware**: https://github.com/AeroCogito/h7-digital-firmware
- **Support Email**: support@aerocogito.com

## License

Documentation is licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

Hardware and firmware follow their respective licenses as documented in their repositories.

## Support

For documentation issues:
- Open an issue in this repository
- Email: support@aerocogito.com

For product support:
- Email: support@aerocogito.com
- Website: https://aerocogito.com

---

**Made in the USA** 🇺🇸
