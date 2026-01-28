# Komet Irrigation — KiCad Common Library

This repository contains the **shared KiCad libraries** used across **Komet Irrigation** hardware projects.  
It is intended to be included **as a Git submodule** to provide a consistent, versioned set of **symbols, footprints, and 3D models**.

---

## Purpose

This repository provides:
- A **single source of truth** for Komet Irrigation KiCad components
- Consistent symbols and footprints across projects
- Reproducible hardware builds via Git submodules
- Clean separation between project design files and shared libraries

---

## Repository Structure


### Structure overview

- **`komet_microcontrollers.kicad_sym`**  
  KiCad symbol library for standalone microcontrollers and SoCs.

- **`komet_microcontrollers.pretty/`**  
  Footprint library associated with microcontrollers.

- **`komet_modules.kicad_sym`**  
  KiCad symbol library for complete modules and assemblies.

- **`komet_modules.pretty/`**  
  Footprint library associated with modules.

- **`3D_models/`**  
  STEP models referenced by footprints for mechanical verification and enclosure checks.

> ⚠️ Only the files and directories listed above are considered **production-ready**.



## Supported KiCad Versions

- **KiCad 9.x** (recommended)
- Earlier versions may not be supported.


## Usage as a Git Submodule (Recommended)

Add this repository as a submodule inside your KiCad project:

```bash
git submodule add <REPO_URL> kicad-libs/komet-kicad
git submodule update --init --recursive
```

## KiCad Configuration

### Path Variable (Required)

To ensure portability across different workstations and CI environments, this library should be referenced using a KiCad path variable.

In **KiCad**: `Preferences → Configure Paths…`


Add a new path variable:

- **Name:** `KOMET_KICAD_LIBS`
- **Path:** Path to this repository or submodule

Recommended value when used as a submodule:

`${PROJECT_DIR}/kicad-libs/komet-kicad`

### Register Symbol Libraries


In **KiCad**:

`Preferences → Manage Symbol Libraries…`


Add the following libraries (Project scope recommended):

- `${KOMET_KICAD_LIBS}/komet_microcontrollers.kicad_sym`
- `${KOMET_KICAD_LIBS}/komet_modules.kicad_sym`

This ensures the libraries are available only to the current project and remain version-controlled.

---

### Register Footprint Libraries

In **KiCad**:

`Preferences → Manage Footprint Libraries…`


Add the following footprint libraries:

- `${KOMET_KICAD_LIBS}/komet_microcontrollers.pretty`
- `${KOMET_KICAD_LIBS}/komet_modules.pretty`

---

### 3D Models

Footprints reference 3D models using relative paths based on the same path variable.

Example:

`${KOMET_KICAD_LIBS}/3D_models/ESP32-PICO-V3.stp`


No additional configuration is required in KiCad.

---

## Naming Conventions

### Symbols

`KOMET_<CATEGORY>_<PART_NAME>`


Examples:
- `KOMET_MCU_ESP32_PICO_V3`
- `KOMET_MODULE_45300139C`

---

### Footprints

- Use the physical part or package name
- Names must match the associated symbol

Examples:
- `ESP32PICOV3`
- `45300139C`

---

### 3D Models

- STEP format (`.stp`)
- File name should match the footprint name where possible

---

## Contribution Guidelines

### Requirements for New Components

Each new component must include:
- A verified **symbol**
- A verified **footprint**
- A referenced **3D model** (or documented justification if unavailable)
- A datasheet reference in the symbol properties



### Quality Checklist

- ✅ Pin numbers and pin types verified
- ✅ Courtyard, fab, and silkscreen layers present
- ✅ Footprint matches datasheet land pattern
- ✅ 3D model alignment verified
- ✅ Naming conventions followed



## Versioning & Releases

- Semantic versioning is recommended:

`v<MAJOR>.<MINOR>.<PATCH>`


- Hardware projects should pin the submodule to a specific tag or commit.

---

## CI / Automation Notes

When used in CI pipelines:
- Initialize submodules before schematic or PCB checks
- Ensure `KOMET_KICAD_LIBS` is defined or paths are relative
- Optional validation steps:
  - missing 3D model files
  - orphaned footprints
  - symbol ↔ footprint mismatches

---

## License & Usage

This repository is intended for **internal use by Komet Irrigation**.  
Third-party models or data remain subject to their respective licenses.

---

## Maintainers

- Hardware Library Maintainers: `Fábio Benjovengo`
- Issues and requests: use the repository issue tracker
