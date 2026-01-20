# RmdGHAutomation

Automated R Markdown to PDF compilation workflow for scientific papers with extensive LaTeX package support.

## Overview

This repository uses GitHub Actions to automatically compile R Markdown (`.Rmd`) documents into PDFs whenever changes are made.  It's designed specifically for scientific and linguistic papers that require extensive LaTeX package support (linguex, qtree, amsmath, etc.).

**Key features:**
- R Markdown for convenient Markdown syntax
- Full LaTeX support for specialized packages
- Automatic PDF compilation on every push
- Works with linguistics packages (linguex, qtree, gb4e)
- Scientific typesetting (amsmath, algorithms, etc.)
- Publisher formats (IEEE, ACM, Springer)

## How It Works

Every time you push changes to `.Rmd` files on the `main` branch or create a pull request, GitHub Actions automatically:

1. Installs R and Pandoc
2. Installs comprehensive TeXLive packages (including linguistics and scientific packages)
3. Renders your R Markdown document using `rmarkdown::render()`
4. Compiles to PDF via LaTeX (pdflatex)
5. Makes both the PDF and intermediate `.tex` file available as downloadable artifacts

### Viewing Workflow Status

- Go to the **Actions** tab in this repository
- Click on the most recent workflow run
- Check if the compilation succeeded (green checkmark) or failed (red X)

## For Authors

### Making Changes to R Markdown Files

1. **Edit the `.Rmd` files** in your local clone or directly on GitHub
2. **Commit and push** your changes to the `main` branch or create a pull request
3. **Wait for the workflow** to run (usually takes 2-3 minutes)
4. **Download the compiled PDF** from the workflow artifacts (see below)

### Downloading Compiled PDFs

To download the compiled PDF after a workflow run:

1. Go to the **Actions** tab
2. Click on the workflow run you want to download from
3. Scroll down to the **Artifacts** section at the bottom of the page
4. Click on **compiled-pdf** to download a zip containing: 
   - `document.pdf` - The final compiled PDF
   - `document.tex` - The intermediate LaTeX file (useful for debugging)

### Using LaTeX Packages in Your R Markdown

You can use any LaTeX package by adding it to the YAML header of your `.Rmd` file:

```yaml
---
title: "Your Paper Title"
output: 
  pdf_document: 
    keep_tex: true
header-includes:
  - \usepackage{linguex}
  - \usepackage{qtree}
  - \usepackage{amsmath}
  - \usepackage{tikz}
  - \usepackage{algorithm2e}
---
```

Then use LaTeX commands directly in your document:

```latex
\ex. This is a linguistic example. 

\Tree [.S [.NP ] [.VP ]]

\begin{equation}
E = mc^2
\end{equation}
```

### Adding More TeXLive Packages

The workflow comes pre-installed with these TeXLive package collections: 
- `texlive-base` - Core LaTeX
- `texlive-latex-recommended` - Common packages
- `texlive-latex-extra` - Additional packages (includes most common needs)
- `texlive-fonts-recommended` - Standard fonts
- `texlive-fonts-extra` - Additional fonts
- `texlive-humanities` - **Linguistics packages (linguex, qtree, gb4e, etc.)**
- `texlive-bibtex-extra` - Bibliography tools
- `texlive-science` - Scientific packages (algorithms, chemistry, physics)
- `texlive-publishers` - Publisher formats (IEEE, ACM, Springer)

If you need additional packages: 

1. Edit `.github/workflows/compile-rmarkdown.yml`
2. Find the "Install TeXLive" step
3. Add the required package to the `apt-get install` command
4. Commit and push the changes

### Finding Missing Packages from Error Logs

If compilation fails with a missing package error: 

1. Go to the **Actions** tab and click on the failed workflow run
2. Click on the **compile-rmarkdown** job
3. Expand the "Render R Markdown to PDF" step
4. Look for errors like `!  LaTeX Error: File 'packagename.sty' not found`
5. Search online for "packagename.sty ubuntu texlive" to find which package to install
6. Common mappings:
   - `algorithm2e. sty` → `texlive-science`
   - `IEEEtran.cls` → `texlive-publishers`
   - `linguex.sty` → `texlive-humanities` (already included)
   - `qtree.sty` → `texlive-humanities` (already included)

### Working with Bibliographies

R Markdown makes bibliographies easy.  Add to your YAML header:

```yaml
---
bibliography: references.bib
citation_package: natbib
---
```

Then cite in your text:  `[@AuthorYear]` or `@AuthorYear`

The workflow will automatically handle BibTeX compilation. 

### R Code Chunks (Optional)

While this template is designed primarily for LaTeX-heavy scientific papers, you can still use R code chunks if needed:

````markdown
```{r my-plot, echo=FALSE, fig.cap="My Figure"}
plot(1:10)
```