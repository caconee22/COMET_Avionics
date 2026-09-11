# COMET_Avionics

COMET rocket avionics hardware project archive.

This repository replaces the previous `COMET_ROK_2` repository with the full
COMET avionics workspace layout.

## Structure

- `ROK2025/`: ROK2-era module board and reference files.
- `ROK2026/`: Current ROK2026 KiCad projects, shared libraries, and revisions.
- `ROK2026/ROK_3/`: ROK3 small-form-factor and SMD-chip test board.
- `ROK2026/ROK_4/`: ROK4 power-design research test board.
- `ROK2026/Away_1/`: Current active avionics candidate version under development.

## Project History

- `ROK2025/COMET_ROK_2`: First ROK2 circuit and module board. It was not used
  because of software development staffing issues.
- `ROK2026/ROK_3`: Miniaturized ROK3 version with SMD chips. It was not used
  because of power-section issues, but it may still be recoverable and remains
  a test version.
- `ROK2026/ROK_4`: A further test version focused on power-section design. It
  is a research version for a small buck-converter power design.
- `ROK2026/Away_1`: The avionics version candidate intended for actual use. It
  is currently under development.

## Avionics Design Intent

The active avionics design intent is documented in
`ROK2026/Away_1/README.md`.

`Away_1` follows the same core direction as the earlier ROK2 avionics/logger
concept: ejection decision and actuation must have the highest priority, and
logging, storage, communication, telemetry, indicators, and console output must
not block that flight-critical path.

Temporary extraction/work folders such as `tmp/` and `tmp_pdf_kmg/` are not
tracked.
