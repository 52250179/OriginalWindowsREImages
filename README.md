# Original Windows RE Images

Unmodified, driver-free **Windows Recovery Environment (WinRE)** images for 64-bit editions of Windows 10 and Windows 11.

## Purpose

These base images serve as a fallback when a live system's `winre.wim` is missing or corrupt.
Deployment scripts can download the required image, inject only the necessary drivers
(e.g., Intel VMD), and deploy a compact, up-to-date recovery environment.

## Source

- **Windows 10 x64** — extracted from a Windows Update-serviced machine running
  Windows 10 22H2 (`10.0.19041.6455`).
- **Windows 11 x64** — extracted from a Windows Update-serviced machine running the
  Windows 11 24H2/25H2 branch (`10.0.26100.9545`).

Both images are pristine at the moment of upload and contain no third-party drivers.
They are WU-serviced states, not ISO baselines: the WinRE servicing level is the one
Windows Update delivered, not the one the installation media shipped.

`Win10/x86/` is reserved for a 32-bit image but is not currently populated. 32-bit
Windows 10 support is not claimed by the consuming deployment scripts.

## Folder structure

```
OriginalWindowsREImages/
├── Win10/
│   └── x64/
│       ├── Winre.7z.001
│       ├── Winre.7z.002
│       └── …
└── Win11/
    ├── Winre.7z.001
    ├── Winre.7z.002
    └── …
```

Archives are split into **25 MB chunks** to stay within GitHub's file size limits for
free plans.

## Usage

1. Download all `Winre.7z.NNN` parts from the required OS folder
   (`Win10/x64/` or `Win11/`).
2. Extract with 7-Zip:
   `7z x Winre.7z.001`
3. Mount or further process the resulting `winre.wim` with DISM.

These images are meant to be consumed automatically by deployment scripts, but manual
extraction is perfectly fine.

## Driver-free invariant

Every image in this repository **must contain zero non-Microsoft driver packages**.
The consuming deployment scripts classify each downloaded source into one of four
ownership classes; a source that contains any non-Microsoft driver and carries no
positive manager-ownership evidence classifies as `Foreign-With-Drivers` and is
preserved rather than normalized. A machine whose only available source is a published
image that fails the invariant therefore refuses to rebuild and defers with
`EXIT_WARNING`.

Before any re-upload, verify the image is clean:

```powershell
# Mount the extracted winre.wim and enumerate driver packages.
Mount-WindowsImage -ImagePath .\winre.wim -Index 1 -Path .\mount
$drivers = @(Get-WindowsDriver -Path .\mount)
$thirdParty = @($drivers | Where-Object {
    $_.ProviderName -and $_.ProviderName -ne 'Microsoft Corporation'
})
Dismount-WindowsImage -Path .\mount -Discard

if ($thirdParty.Count -gt 0) {
    Write-Host "NOT CLEAN - do not publish:" -ForegroundColor Red
    $thirdParty | Format-List Driver, ProviderName, Version, OriginalFileName
} else {
    Write-Host "CLEAN - safe to publish" -ForegroundColor Green
}
```

A non-zero count means the source has been serviced by a third-party driver — including
the manager's own prior injection on a machine whose registered WinRE was captured for
upload. Do not publish such an image.

## Size limitation note

File chunks are capped at **25 MB** because GitHub restricts individual file sizes on
free accounts. This does not affect extraction — 7-Zip seamlessly reassembles the
original `.wim`.

## Licensing

These files are derived from Microsoft Windows. All rights remain with Microsoft.
Redistribution is permitted only for recovery purposes on properly licensed systems.
