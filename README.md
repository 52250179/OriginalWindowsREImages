# Original Windows RE Images

Unmodified, driver-free **Windows Recovery Environment (WinRE)** images for the 64-bit editions of Windows 10 and Windows 11, plus a 32-bit Windows 10 image for manual or third-party consumption.

## Purpose

These base images serve as a fallback when a live system's `winre.wim` is missing or corrupt.
Deployment scripts can download the required image, inject only the necessary drivers
(e.g., Intel VMD), and deploy a compact, up-to-date recovery environment.

## Source

- **Windows 10 x64** — extracted from a Windows Update-serviced machine running
  Windows 10 22H2 (`10.0.19041.6455`).
- **Windows 10 x86** — extracted from the same Windows Update-serviced machine,
  same servicing level (`10.0.19041.6455`).
- **Windows 11 x64** — extracted from a Windows Update-serviced machine running the
  Windows 11 24H2/25H2 branch (`10.0.26100.9545`).

All three images are pristine at the moment of upload and contain no third-party drivers.
They are WU-serviced states, not ISO baselines: the WinRE servicing level is the one
Windows Update delivered, not the one the installation media shipped.

These archives are refreshed on the maintainer's cadence and may lag the latest
Windows Update-delivered WinRE servicing level by some interval. A machine that has
been serviced by WU more recently than the archive was captured may have a
registered WinRE that is newer than the published fallback. The consuming
deployment scripts select sources by DISM servicing metadata and prefer the target
machine's registered WinRE when it is at least as new as the last-known-good copy;
the published archives are the fallback of last resort, not the primary source.
If a newer servicing level is required, capture the current machine's `winre.wim`
and use it as the source instead.

## Folder structure
