# Upstream source

These files are copied from TinyQV by Michael Bell, Apache-2.0 (see LICENSE in this folder).

- Repo: https://github.com/MichaelBell/tinyQV
- Branch: ttsky25a (the version taped out on Tiny Tapeout SKY25a)
- Commit: baf7d4dc6fbdd7d348fa146c6bea1d6f6e03e664 (3 Sep 2025)

Copied: cpu/ (all RTL used by the core; latch_reg.v and qspi_flash.v left out as unused),
test/ (cocotb testbenches, minus the legacy qspi_flash test), verify/ (formal + lint).

Local changes:
- verify/lint.sh: dropped ../peri/*/*.v (peripherals not copied).
