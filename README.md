# FCJ Workshop Template - Internship Report

A comprehensive Hugo-based documentation template for FCJ (FPT Cloud Intern) internship reports and workshop materials at Amazon Web Services Vietnam.

**📍 Live Demo: [View Report](https://superziggs.github.io/Internship-report/)**

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation & Setup](#installation--setup)
- [Usage](#usage)
- [Content Structure](#content-structure)
- [Configuration](#configuration)
- [Deployment](#deployment)
- [Languages](#languages)
- [Author](#author)
- [License](#license)

## 🎯 Overview

This project serves as an internship report documentation portal for the FCJ (FPT Cloud Intern) program at AWS Vietnam. It showcases:

- **12-week internship worklog** with detailed weekly reports
- **Project proposals** and documentation
- **Translated technical blogs** and learning materials
- **Events and workshops** participation records
- **Hands-on workshop guides** and tutorials
- **Self-evaluation** and feedback sections

**Student:** Le Nguyen Thien Danh  
**Position:** FCJ Cloud Intern  
**Company:** Amazon Web Services Vietnam Co., Ltd.  
**Duration:** From January 5, 2026  
**Major:** Artificial Intelligence (FPT University)

## ✨ Features

- **Bilingual Support**: Full Vietnamese and English translations (i18n)
- **Hugo Theme Learn**: Clean, professional learning-focused theme
- **Multi-layered Content**: Organized hierarchical structure for easy navigation
- **SEO Optimized**: RSS feed, JSON output, and canonical URLs
- **GitHub Integration**: Direct edit links for GitHub-based contributions
- **Search Functionality**: Built-in JSON search support
- **Responsive Design**: Mobile-friendly layout with custom theming

## 📁 Project Structure

```
fcj-workshop-template/
├── content/                          # Main content directory
│   ├── _index.md                    # Homepage (English)
│   ├── _index.vi.md                 # Homepage (Vietnamese)
│   ├── 1-Worklog/                   # 12-week worklog entries
│   │   ├── 1.1-Week1/
│   │   ├── 1.2-Week2/
│   │   └── ... (up to Week 12)
│   ├── 2-Proposal/                  # Project proposal
│   ├── 3-BlogsTranslated/           # Technical blogs
│   ├── 4-EventParticipated/         # Event participation
│   ├── 5-Workshop/                  # Hands-on workshop guides
│   │   ├── 5.1-Workshop-overview/
│   │   ├── 5.2-Setup/
│   │   ├── 5.3-Run-RealityCapture/
│   │   ├── 5.4-Cleanup/
│   │   └── 5.5-Learn-More/
│   ├── 6-Self-evaluation/           # Self-assessment
│   └── 7-Feedback/                  # Feedback section
├── layouts/                         # Custom Hugo layouts
│   ├── partials/                   # Reusable components
│   └── shortcodes/                 # Custom shortcodes (tabs, etc.)
├── static/                         # Static assets
│   ├── css/                        # Custom stylesheets
│   ├── fonts/                      # Web fonts
│   └── images/                     # Image assets
├── themes/                         # Hugo theme directory
│   └── hugo-theme-learn/
├── archetypes/                     # Content templates
├── config.toml                     # Hugo configuration
└── _index2.md                      # Additional index
```

## 📋 Prerequisites

- **Hugo** (Extended version recommended) - [Install Hugo](https://gohugo.io/installation/)
- **Git** - for version control
- **Text Editor** - VS Code or any code editor

### Recommended Versions

- Hugo: v0.100.0 or later
- Node.js: v14 or later (for additional asset processing if needed)

## 🚀 Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/SuperZiggs/Internship-report.git
cd Internship-report
```

### 2. Initialize Submodules

The theme is included as a Git submodule:

```bash
git submodule update --init --recursive
```

### 3. Install Hugo

If not already installed:

```bash
# macOS (using Homebrew)
brew install hugo

# Windows (using Chocolatey)
choco install hugo-extended

# Or download from https://github.com/gohugoio/hugo/releases
```

### 4. Verify Installation

```bash
hugo version
```

## 📖 Usage

### Local Development

Start the Hugo development server:

```bash
hugo server
```

The site will be available at `http://localhost:1313`

### With Draft Posts

To include draft content:

```bash
hugo server -D
```

### Build for Production

Generate the static site:

```bash
hugo
```

The compiled site will be in the `public/` directory.

## 📑 Content Structure

### Adding New Content

Use Hugo's content creation command:

```bash
# Create a new page
hugo new path/to/new-page/_index.md
```

### File Naming Convention

- All section pages use `_index.md` (English) and `_index.vi.md` (Vietnamese)
- Bilingual content is organized in the same directory

### Front Matter Example

```yaml
---
title: "Page Title"
date: 2026-03-20
weight: 1
chapter: false
---
```

## ⚙️ Configuration

Edit `config.toml` to customize:

- **Base URL**: Change `baseURL` for your deployment domain
- **Theme**: Already set to `hugo-theme-learn`
- **Languages**: Configured for English and Vietnamese
- **Edit URL**: Points to GitHub repository for inline editing
- **Theme Variant**: Set to "workshop" (can be changed to "red", "blue", or "green")

### Key Configuration Sections

```toml
# Base configuration
baseURL = "https://superziggs.github.io/Internship-report/"
theme = "hugo-theme-learn"
defaultContentLanguage = "en"

# Parameters
[params]
  themeVariant = "workshop"
  editURL = "https://github.com/SuperZiggs/Internship-report/edit/main/content/"
  author = "SuperZiggs"
```

## 🌐 Languages

The site is configured for **bilingual support**:

### English (Default)

- Language code: `en`
- Weight: 1
- Title: "Internship Report"
- Files: `_index.md` and all content pages

### Vietnamese

- Language code: `vi`
- Weight: 2
- Title: "Báo cáo thực tập"
- Files: `_index.vi.md` and equivalent content pages

**Navigation:** Language switcher in the top menu allows users to toggle between English and Vietnamese versions.

## 🚀 Deployment

### GitHub Pages

The site is configured to deploy to GitHub Pages:

1. Build the site:
   ```bash
   hugo
   ```

2. Push the `public/` directory to your GitHub repository's `gh-pages` branch

3. Access at: `https://superziggs.github.io/Internship-report/`

### Alternative Hosting Platforms

This Hugo site can be deployed to:

- **Netlify** (recommended)
- **Vercel**
- **AWS Amplify**
- **GitLab Pages**
- **Self-hosted servers**

## 📚 Additional Resources

- **Hugo Documentation**: https://gohugo.io/documentation/
- **Theme Documentation**: See `themes/hugo-theme-learn/README.md`
- **AWS Study Group**: https://www.facebook.com/groups/awsstudygroupfcj/

## 👤 Author

**Le Nguyen Thien Danh**

- **Email**: dinhloc.truong2005@gmail.com
- **University**: FPT University
- **Major**: Artificial Intelligence
- **Current Role**: FCJ Cloud Intern at AWS Vietnam

## 📄 License

This project is based on the Hugo Learn theme. For licensing information, see:

- Main theme: `themes/hugo-theme-learn/LICENSE.md`
- Custom content: Check GitHub repository for specific licenses


## 📞 Support

For issues or questions:

- **Email**: dinhloc.truong2005@gmail.com

---

