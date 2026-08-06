# TIFF, GeoTIFF, and Cloud Optimized GeoTIFF (COG)

A Jupyter Book explaining the TIFF, GeoTIFF, and Cloud Optimized GeoTIFF (COG) formats.

## Overview

This book provides a comprehensive introduction to:

- **TIFF**: The Tagged Image File Format, covering structure and basic metadata.
- **GeoTIFF**: TIFF extended with geospatial metadata for coordinate systems and georeferencing.
- **Cloud Optimized GeoTIFF (COG)**: A cloud-optimized variant designed for efficient streaming and partial access over HTTP.

## Building the Book

### Prerequisites

- Python 3.7+
- Jupyter Book

### Installation

Install dependencies from `requirements.txt`:

```bash
pip install -r requirements.txt
```

### Build

Build the HTML version:

```bash
jupyter-book build book
```

The generated HTML will be in `book/_build/html/`. Open `book/_build/html/index.html` in a browser to view the book.

## Project Structure

```
.
├── README.md              # This file
├── requirements.txt       # Python dependencies
├── book/                  # Jupyter Book source directory
│   ├── _config.yml        # Jupyter Book configuration
│   ├── _toc.yml           # Table of contents
│   ├── intro.md           # Introduction
│   ├── tiff.md            # TIFF format documentation
│   ├── geotiff.md         # GeoTIFF format documentation
│   ├── cog.md             # Cloud Optimized GeoTIFF documentation
│   ├── references.bib     # Bibliography (BibTeX format)
│   ├── images/            # Image assets for the book
│   └── _build/            # Generated build output
```

## Contributing

To edit or add content:

1. Modify the relevant `.md` file
2. Rebuild the book: `jupyter-book build book`
3. View changes in `book/_build/html/`

For cross-references between pages, use MyST `{ref}` syntax:

```markdown
{ref}`Link text <target-label>`
```

Target labels are defined with the syntax:

```markdown
(target-label)=
## Section Heading
```

## Notes

- The `book/images/README.md` files are not included in the book TOC and are internal documentation. Build warnings about this are expected and can be safely ignored.
