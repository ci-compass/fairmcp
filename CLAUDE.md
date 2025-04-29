# FAIRMCP Project Guidelines

## Project Overview

FAIRMCP is a GitHub repository that provides a Model Context Protocol (MCP) server for FAIR documentation. The repository combines:
- FAIR principles (Findable, Accessible, Interoperable, Reusable) documentation
- Model Context Protocol (MCP) server implementation
- Quarto-based documentation site deployed on GitHub Pages
- Schema.org metadata integration

## Claude Code References
The llmstxt folder has markdown references that may be of use to claude code.Ok

## Setup & Environment

### Package Management
- Use `uv` for Python package management
- Install packages: `uv pip install <package-name>`
- Create virtual environment: `uv venv`
- Activate virtual environment: `source .venv/bin/activate` (bash/zsh) or `.venv\Scripts\activate` (Windows)

### Installation Commands
# Install Quarto CLI (required for building the site)/
Quarto is installed via homebrew on macOS:
```bash
brew install --cask quarto
```

# Create and activate Python environment
uv venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install required packages
uv add jupyter
uv add pytest

# Sync requirements
uv sync


## Proposed Repository Structure -- This will change

```
fairmcp/
├── .github/                    # GitHub-specific files
│   └── workflows/              # GitHub Actions for Quarto builds
├── _quarto.yml                 # Quarto configuration file
├── bibliography.bib            # Bibliography file for citations
├── index.qmd                   # Home page
├── llms.txt                    # Entry point for LLMs 
├── docs/                       # Documentation
│   ├── principles/             # FAIR principles documentation
│   │   ├── _index.qmd          # Section landing page
│   │   ├── findable.qmd        
│   │   ├── accessible.qmd
│   │   ├── interoperable.qmd
│   │   └── reusable.qmd
│   ├── implementations/        # Implementation guides
│   │   ├── _index.qmd
│   │   ├── metadata.qmd
│   │   ├── vocabularies.qmd
│   │   └── ...
│   ├── tools/                  # Tools and resources
│   └── examples/               # Examples
├── mcp/                        # MCP server implementation
│   ├── tools/                  # MCP tools implementation
│   └── handlers/               # Request handlers
├── scripts/                    # Python scripts for site building
│   └── generate_llms_txt.py    # Script to generate llms.txt
├── _site/                      # Generated site (not versioned)
├── LICENSE                     # License file (recommend CC-BY)
└── README.md                   # Repository overview
```

## Quarto Configuration Guidelines

### QMD File Format

QMD files should include frontmatter with schema.org metadata like this:

```yaml
---
title: "Making Vocabularies FAIR"
author: "FAIRMCP Team"
format: 
  html: 
    toc: true
    html-math-method: katex
    css: styles.css
bibliography: ../../bibliography.bib
metadata:
  json-ld: |
    {
      "@context": "https://schema.org/",
      "@type": "TechArticle",
      "name": "Making Vocabularies FAIR",
      "author": {
        "@type": "Person",
        "name": "FAIRMCP Team"
      },
      "about": {
        "@type": "DefinedTerm",
        "name": "FAIR Principles",
        "url": "https://www.go-fair.org/fair-principles/"
      }
    }
---
```

### Required Quarto Configuration

The `_quarto.yml` file should include:

```yaml
project:
  type: website
  output-dir: _site
  
website:
  title: "FAIR MCP Documentation"
  site-url: "https://ci-compass.github.io/fairmcp"
  description: "FAIR Principles Documentation with Model Context Protocol Support"
  navbar:
    left:
      - href: index.qmd
        text: Home
      - href: docs/principles/_index.qmd
        text: FAIR Principles
      - href: docs/implementations/_index.qmd
        text: Implementation
        
format:
  html:
    theme: cosmo
    css: styles.css
    toc: true
    code-fold: true
    code-tools: true
  md:
    output-file: "{{file}}.md"
    
execute:
  enabled: true
  cache: true
  freeze: auto
    
bibliography: bibliography.bib
csl: ieee.csl
```

## MCP Implementation

The MCP server should implement these tools:

1. `fetch_fair_documentation`: Retrieve primary documentation about FAIR principles
2. `search_fair_documentation`: Enable semantic search within the FAIR documentation
3. `fetch_fair_examples`: Return concrete examples of FAIR implementation
4. `search_fair_code`: Search through implementation examples and code snippets

## Important Commands

```bash
# Build the site (HTML and CommonMark formats)
quarto render

# Preview the site locally
quarto preview

# Run tests
pytest

# Update llms.txt index for LLMs
python scripts/generate_llms_txt.py

# Build only CommonMark files
quarto render --to commonmark
```

## CommonMark and llms.txt

The repository uses CommonMark format for LLM-friendly documentation, following the [llms.txt specification](https://llmstxt.org/).

When you run `quarto render`, all Quarto documents are rendered to both HTML (for humans) and CommonMark (for LLMs). The CommonMark files are named with the original filename followed by `.md`.

The `generate_llms_txt.py` script creates an index of all available markdown documentation in the repository, which is saved as `llms.txt` in the repository root. This serves as an entry point for LLMs to discover and navigate the documentation.

## Style Guidelines

- Use American English spelling consistently
- Use active voice where possible
- Include code examples for implementation guidance
- Add citations using `@citation-key` format
- Ensure all acronyms are defined on first use
- Include schema.org metadata in all content files

## Citation Guidelines

When citing papers, use the following format in the text:

```markdown
According to @wilkinson2016fair, the FAIR principles...
```

Ensure all citations have corresponding entries in the `bibliography.bib` file.

## GitHub Pages Deployment

The site is automatically deployed to GitHub Pages using GitHub Actions when changes are pushed to the main branch. The workflow should:

1. Install Quarto
2. Render the site
3. Generate .md versions of all pages
4. Generate and update llms.txt
5. Deploy to the gh-pages branch

## Metadata Standards

Schema.org metadata should follow these types where appropriate:
- ScholarlyArticle for academic content
- TechArticle for technical guides
- DefinedTerm for FAIR principles definitions
- SoftwareSourceCode for code examples

## Testing

Write unit tests for all MCP tools and Python scripts. Test coverage should be at least 80%.