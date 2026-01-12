# GitBook Documentation Setup

This folder contains the GitBook documentation for Overflow Development resources.

## Structure

```
docs/
├── README.md                    # Main welcome page with dev greeting and terms
├── SUMMARY.md                   # Table of contents
├── book.json                    # GitBook configuration
├── .gitbook.yaml               # GitBook YAML config
├── overflow-progressbar/        # Progress bar intro
├── getting-started/            # Installation and quick start
├── configuration/              # Configuration guides
├── api/                        # API reference
├── examples/                   # Code examples
├── advanced/                   # Advanced topics
├── troubleshooting/            # Troubleshooting guides
└── resources/                  # Reference materials
```

## Building the Documentation

### Option 1: GitBook CLI (Legacy)

1. Install GitBook CLI:
   ```bash
   npm install -g gitbook-cli
   ```

2. Install plugins:
   ```bash
   cd docs
   gitbook install
   ```

3. Serve locally:
   ```bash
   gitbook serve
   ```

4. Build static site:
   ```bash
   gitbook build
   ```

### Option 2: GitBook.com (Recommended)

1. Sign up at [gitbook.com](https://www.gitbook.com/)
2. Connect your GitHub repository
3. GitBook will automatically build and host your documentation

### Option 3: HonKit (Modern Alternative)

HonKit is the modern fork of GitBook CLI with better maintenance:

1. Install HonKit:
   ```bash
   npm install -g honkit
   ```

2. Serve locally:
   ```bash
   honkit serve
   ```

3. Build static site:
   ```bash
   honkit build
   ```

## Viewing Locally

After running `gitbook serve` or `honkit serve`, open:
```
http://localhost:4000
```

## Publishing Options

### GitHub Pages

1. Build the documentation:
   ```bash
   gitbook build
   ```

2. The output will be in `_book/` folder
3. Push `_book/` contents to `gh-pages` branch
4. Enable GitHub Pages in repository settings

### Netlify/Vercel

1. Build command: `gitbook build` or `honkit build`
2. Publish directory: `_book`
3. Deploy automatically on git push

### GitBook.com Hosting

1. Import repository to GitBook.com
2. Automatic builds and hosting
3. Custom domain support
4. Analytics and insights

## Customization

### Theme

Edit `book.json` to customize:
- Title and description
- Plugins
- Theme settings
- Variables

### Styling

Add custom CSS:
1. Create `styles/website.css`
2. Add custom styles
3. Rebuild

### Plugins

Popular plugins in `book.json`:
- `search` - Search functionality
- `github` - GitHub integration
- `prism` - Code syntax highlighting
- `copy-code-button` - Copy button for code blocks
- `expandable-chapters` - Collapsible chapters

## Important Files

### SUMMARY.md

This file defines the Table of Contents structure. GitBook uses this to generate the sidebar navigation.

### book.json

Configuration file for:
- Metadata (title, author, description)
- Plugins and their settings
- Output formats (PDF, ePub)
- Custom variables

### .gitbook.yaml

Modern GitBook configuration for GitBook.com platform.

## Documentation Standards

### File Naming

- Use lowercase with hyphens: `getting-started.md`
- Be descriptive: `installation.md` not `install.md`
- Group related files in folders

### Markdown Style

- Use proper heading hierarchy (H1, H2, H3)
- Include code examples with language tags
- Add Table of Contents for long pages
- Use relative links between pages

### Code Blocks

Always specify the language:

\`\`\`lua
-- Lua code here
\`\`\`

\`\`\`bash
# Bash commands here
\`\`\`

### Links

Use relative links for internal navigation:
```markdown
[Installation](../getting-started/installation.md)
```

## Maintenance

### Adding New Pages

1. Create the markdown file
2. Add entry to `SUMMARY.md`
3. Rebuild documentation

### Updating Content

1. Edit the markdown files
2. Commit and push changes
3. Documentation auto-rebuilds (on GitBook.com)

### Version Control

- Keep documentation in sync with code version
- Tag releases in git
- Update `Changelog.md` with each release

## License Notice

All documentation is Copyright © 2024 Overflow Development (@Overflow_ID).

Unauthorized copying or distribution is prohibited. See main [README.md](README.md) for full terms.

## Support

For documentation issues:
- 📧 Email: support@overflow-dev.id
- 🐛 GitHub Issues

---

**Overflow Development Documentation Team**
