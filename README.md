Markdown-based CV

Special thanks to (elipapa's project)[https://github.com/elipapa/markdown-cv] as my inspiration and reference.

## Quick Overview
This CV is using Jekyll-based page and renders by the GitHub page pipeline.

## Pipeline Flow

```mermaid
graph LR;
    A[Generate GitHub Pages]-->B[Create PDF file]
    B[Create PDF file]-->C[Setup ENV]
    C[Setup ENV]-->D[Create Release Tag]
    D[Create Release Tag]-->E[Upload Release Asset]
```