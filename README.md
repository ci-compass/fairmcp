# fairmcp
MCP FAIR Documentation Repository for FAIR Agents

## About
This repository provides a Model Context Protocol (MCP) server for FAIR documentation. The repository combines FAIR principles (Findable, Accessible, Interoperable, Reusable) documentation with a Model Context Protocol (MCP) server implementation.

## LLM-friendly Documentation
This repository follows the [llms.txt specification](https://llmstxt.org/). When the site is built, all Quarto documents are also rendered to CommonMark format, making them accessible to Large Language Models.

You can find the LLM-friendly index at `/llms.txt` which provides an overview of available documentation.

## Setup

### Prerequisites
- [Quarto](https://quarto.org/docs/get-started/)
- Python 3.10+
- uv (Python package manager)

### Installation
```bash
# Create and activate Python environment
uv venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install required packages
uv pip install -r requirements.txt
```

### Building the Documentation
```bash
# Render the site (HTML and CommonMark formats)
quarto render

# Generate the llms.txt index
python scripts/generate_llms_txt.py

# Preview the site locally
quarto preview
```
