# Web Source Code Downloader & Crawler

<div align="right">

[![Actions Status](https://github.com/SagarBiswas-MultiHAT/WebSource-Harvester/actions/workflows/get-started-with-github-actions.yml/badge.svg)](https://github.com/SagarBiswas-MultiHAT/WebSource-Harvester/actions) 
&nbsp;
[![License](https://img.shields.io/github/license/SagarBiswas-MultiHAT/WebSource-Harvester.svg)](https://github.com/SagarBiswas-MultiHAT/WebSource-Harvester/blob/main/LICENSE) 
&nbsp;
[![Last Commit](https://img.shields.io/github/last-commit/SagarBiswas-MultiHAT/WebSource-Harvester.svg)](https://github.com/SagarBiswas-MultiHAT/WebSource-Harvester/commits) 
&nbsp;
[![Open Issues](https://img.shields.io/github/issues/SagarBiswas-MultiHAT/WebSource-Harvester.svg)](https://github.com/SagarBiswas-MultiHAT/WebSource-Harvester/issues) 
&nbsp;
[![Top Language](https://img.shields.io/github/languages/top/SagarBiswas-MultiHAT/WebSource-Harvester.svg)](https://github.com/SagarBiswas-MultiHAT/WebSource-Harvester)

</div>

**Author:** @SagarBiswas-MultiHAT \
**Category:** Educational Web Crawling & Client-Side Security Analysis \
**Status:** Learning-grade, interview-safe, portfolio-ready

> “This project performs depth-controlled crawling and client-side source reconstruction, capturing everything a browser can observe from a given URL, while intentionally respecting server-side trust boundaries.”

---

![](https://imgur.com/WS98QtD.png)

<br>

---

![after running the project](https://imgur.com/luk0aqV.png)

<br>

---

#### Tested example: python ".\PasourceDownloader.pyssword-Strength-Checker" https://sagarbiswas-multihat.github.io/ --depth 2

<br>

<div align="center">
    
![downloaded elements](https://imgur.com/8yCIRkQ.png)

</div>

<br>

---

## Overview

This project is an **educational website source code downloader and crawler** that extracts and reconstructs **everything a browser can observe** from a given URL.

It crawls a site with depth-controlled **BFS**, downloads client-visible resources (HTML, CSS, JS, images, fonts, PDFs, etc.), and rewrites links so the pages work **offline**, even on nested paths like `/blog/*`.

The tool **respects server trust boundaries** and does **not** attempt to fetch backend code, databases, or private data.

---

## What this project provides (accurate scope)

This tool captures **everything a browser can retrieve** from a URL:

- HTML pages (multiple pages via crawling)
- Linked CSS files
- JavaScript files
- Images (including `srcset`)
- Fonts and media files
- PDFs and other static assets
- XML files (e.g., `sitemap.xml`)
- Correct offline reconstruction via path rewriting

Ideal for:

- Learning how real websites are structured
- Offline inspection and analysis
- Client-side security research
- Understanding exposure and attack surface
- Portfolio demonstrations of crawling logic

---

## What this project intentionally does NOT do

By design, this project does **not**:

- Download backend source code (PHP, Python, Node.js, etc.)
- Access databases or APIs that require authentication
- Execute JavaScript (SPA/React/Vue rendering)
- Bypass authentication, paywalls, or access controls
- Retrieve secrets, tokens, or server configuration

These limitations are **intentional** and make the project accurate and interview-safe.

---

## Key features

- **Depth-controlled crawling** (`--depth 2` or `--depth 1-2`)
- **Breadth-First Search (BFS)** for reliable depth measurement
- **Crawl vs analyze separation** to control what gets saved
- **Same-origin enforcement** (no external domain crawling)
- **Offline-safe path rewriting** for nested pages
- **Asset handling** for `src`, `href`, and `srcset`
- **URL decoding** (`%20` → spaces)
- **Query collision handling** via hash suffix
- **Content-type aware saving** for missing extensions
- **XML-aware parsing** for sitemaps and RSS

---

## How depth works (`--depth`)

Depth is measured in **link hops** from the base URL:

- `0` → only the base URL
- `1` → base URL + pages directly linked from it
- `2` → links from depth-1 pages
- `1-2` → crawl broadly, analyze only depth 1–2

Example structure:

```
Depth 0
└── https://example.com

Depth 1
├── /about
├── /blog
└── /login

Depth 2
├── /blog/post-1
├── /blog/post-2
└── /about/team
```

Depth control reduces noise and focuses on pages where real-world issues usually live.

---

## Installation

Recommended: use a virtual environment.

```bash
python -m venv .venv
```

Activate:

**Windows (PowerShell):**

```bash
.venv\Scripts\Activate.ps1
```

**Linux / macOS:**

```bash
source .venv/bin/activate
```

Install dependencies:

1. Install from the bundled `requirements.txt` (recommended):

```bash
pip install -r requirements.txt
```

2. Or install packages individually (equivalent):

```bash
pip install beautifulsoup4 lxml
```

Notes:

- `lxml` is optional but recommended (it provides a robust XML parser and removes XML parsing warnings).
- Use the Python provided in your `.venv` when running the `pip` command to ensure packages install into the virtual environment.

---

## Usage

```bash
python "web-source-code_downloader" <BASE_URL> --depth <DEPTH>
```

Examples:

```bash
python "web-source-code_downloader" https://example.com --depth 0
python "web-source-code_downloader" https://example.com --depth 1
python "web-source-code_downloader" https://example.com --depth 1-2
```

---

## Output structure (example)

```
example_com/
├── index.html
├── assets/
│   ├── css/
│   ├── js/
│   ├── images/
│   └── fonts/
└── blog/
    ├── post-1.html
    └── post-2.html
```

All pages open **offline** without broken CSS or images.

---

## Why nested pages work correctly

Many crawlers rewrite assets relative to the project root, which breaks pages like `/blog/post.html`.

This project rewrites assets **relative to each HTML file’s directory**, so both root and nested pages load properly.

---

## Output behavior (important)

- **Assets are saved as binary**, so images and PDFs stay intact.
- **Pages are saved as HTML**, with rewritten local paths.
- **External URLs** (GitHub badges, CDNs) are kept external.
- **Fragment-only links** (`#about`) are ignored to reduce crawl noise.

---

## Troubleshooting

- **Images missing on nested pages** → fixed by file-relative rewriting
- **Responsive images missing** → `srcset` entries are downloaded and rewritten
- **Resume/PDF not opening** → binary assets are saved directly
- **XML warnings** → install `lxml` or ignore (HTML fallback is handled)

---

## Ethical & Legal Notice

This tool is for **educational and authorized testing only**. Always respect terms of service and robots policies.

---

## Future improvements (optional)

- Headless rendering (Playwright) for JS-heavy sites
- robots.txt enforcement
- JSON crawl reports
- Security header analysis
- Rate limiting and concurrency
- Authentication support for authorized environments

---

## Final note

This project is designed to be **honest, technically correct, and impressive without exaggeration**. It demonstrates strong understanding of web architecture, crawling logic, and security boundaries.
