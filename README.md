# Original Windows RE Images

Unmodified, driver‑free **Windows Recovery Environment (WinRE)** images for 64‑bit editions of Windows 10 and Windows 11.

## Purpose
These base images serve as a fallback when a live system’s WinRE.wim is missing or corrupt.  
Deployment scripts can download the required image, inject only the necessary drivers (e.g., Intel VMD), and deploy a compact, up‑to‑date recovery environment.

## Source
- **Windows 10** – extracted from an official Windows 10 22H2 ISO.
- **Windows 11** – extracted from an official Windows 11 25H2 ISO.  
Both images are pristine and contain no third‑party drivers.

## Folder Structure
OriginalWindowsREImages/
├── Win10/
│ ├── winre.wim.7z.001
│ ├── winre.wim.7z.002
│ └── …
└── Win11/
├── winre.wim.7z.001
├── winre.wim.7z.002
└── …

Archives are split into **25 MB chunks** to stay within GitHub’s file size limits for free plans.

## Usage
1. Download all parts from the required OS folder.
2. Extract with 7‑Zip:  
   `7z x winre.wim.7z.001`
3. Mount or further process the resulting `winre.wim` with DISM.

These images are meant to be consumed automatically by deployment scripts, but manual extraction is perfectly fine.

## Size Limitation Note
File chunks are capped at **25 MB** because GitHub restricts individual file sizes on free accounts.  
This does not affect extraction – 7‑Zip seamlessly reassembles the original `.wim`.

## Licensing
These files are derived from Microsoft Windows. All rights remain with Microsoft.  
Redistribution is permitted only for recovery purposes on properly licensed systems.
