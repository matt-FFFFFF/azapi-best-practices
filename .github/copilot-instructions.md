# AzAPI Best Practices Documentation Site

AzAPI Best Practices is a Hugo-based static documentation website that provides comprehensive guidance for using the AzAPI Terraform provider effectively. The site uses the hugo-book theme and is deployed to GitHub Pages.

Always reference these instructions first and fallback to search or bash commands only when you encounter unexpected information that does not match the info here.

## Working Effectively

### Initial Setup and Build Process
- Install Hugo Extended (CRITICAL: v0.146.0+ required, despite GitHub workflow using v0.120.4):
  ```bash
  wget -O /tmp/hugo.deb https://github.com/gohugoio/hugo/releases/download/v0.146.0/hugo_extended_0.146.0_linux-amd64.deb
  sudo dpkg -i /tmp/hugo.deb
  hugo version  # Verify installation
  ```
- Initialize git submodules (required for theme):
  ```bash
  git submodule update --init --recursive
  ```
- Build the site:
  ```bash
  hugo --minify
  ```
  Build time: ~200ms (extremely fast). NEVER CANCEL - builds complete in under 1 second.

### Development Workflow
- Start development server:
  ```bash
  hugo server
  ```
  Available at: http://localhost:1313/azapi-best-practices/
  Build time: ~200ms. NEVER CANCEL - server starts in under 3 seconds.
- The server supports hot reload for content changes

### Production Build
- Build for production:
  ```bash
  hugo --minify
  ```
  Output directory: `public/`
  Build time: ~200ms. NEVER CANCEL - completes in under 1 second.

## Validation

### Manual Testing Scenarios
ALWAYS run through these validation scenarios after making changes:

1. **Homepage Navigation**:
   - Navigate to http://localhost:1313/azapi-best-practices/
   - Verify homepage loads with sidebar navigation and main content
   - Check that "Get Started →" link works

2. **Content Navigation**:
   - Test all sidebar navigation links (Getting Started, Understanding AzAPI, Core Concepts, etc.)
   - Verify table of contents (TOC) in right sidebar updates per page
   - Test "Next" navigation links at bottom of pages

3. **Search Functionality**:
   - Enter search terms in the search box (e.g., "authentication")
   - Verify search results appear with relevant pages
   - Test clicking on search results

4. **Content Rendering**:
   - Verify code blocks display with syntax highlighting
   - Check that Mermaid diagrams render (decision flow diagrams)
   - Ensure responsive design works on different screen sizes

5. **Theme Features**:
   - Test dark/light theme switching (auto mode by default)
   - Verify all fonts and styling load correctly

### Expected Warnings
The build process generates these warnings (normal and expected):
- Reference warnings for missing pages (performance/, patterns/, troubleshooting/)
- Mermaid diagram warnings in console (functionality still works)

## Architecture and Structure

### Key Components
- **Content**: All documentation in `content/docs/` directory
- **Theme**: Hugo-book theme loaded via git submodule at `themes/hugo-book/`
- **Configuration**: `hugo.toml` contains site configuration
- **GitHub Actions**: `.github/workflows/hugo.yml` handles automatic deployment

### Content Structure
```
content/
├── _index.md              # Homepage
└── docs/
    ├── _index.md          # Documentation index
    ├── getting-started.md # Getting started guide
    ├── understanding-azapi.md
    ├── core-concepts.md
    ├── resource-management.md
    ├── security-compliance.md
    └── testing.md
```

### Version Compatibility Issue
**CRITICAL**: README.md specifies Hugo v0.146.0+ but GitHub workflow uses v0.120.4. 
- For local development: use v0.146.0+
- For CI compatibility: consider updating workflow to match README requirements

## Common Tasks

### Adding New Content
- Create new `.md` files in `content/docs/`
- Follow existing frontmatter structure
- Add appropriate weight for ordering
- Update navigation if needed

### Troubleshooting
- If build fails with template errors, check Hugo version compatibility
- If submodules are missing, run `git submodule update --init --recursive`
- If search doesn't work, ensure site builds completely and JavaScript loads
- For styling issues, check if theme submodule is properly initialized

### No Testing/Linting Infrastructure
- This is a documentation site with no automated tests
- No linting tools are configured
- Validation relies entirely on manual testing scenarios above
- Content accuracy depends on manual review

## Repository Quick Reference

### Key Files
```bash
# Configuration
cat hugo.toml              # Hugo configuration
cat README.md              # Project overview and setup instructions
cat .gitmodules           # Git submodule configuration

# Content
ls content/docs/          # All documentation files
ls layouts/               # Custom layout overrides (if any)

# Build output  
ls public/               # Generated site (after hugo build)
```

### GitHub Actions Workflow
- Automatically deploys to GitHub Pages on push to main
- Uses Hugo v0.120.4 (differs from local requirement)
- Builds with `hugo --minify`
- Uploads to GitHub Pages artifact

### Site Features
- Book-style layout with sidebar navigation
- Full-text search functionality
- Mermaid diagram support
- Mobile responsive design
- Dark/light theme support (auto-switching)
- Syntax highlighting for code blocks
- Table of contents generation