# MPX NYC: Reproducible Research Repository [under development]

> Source code and analytic workflow for the MPX NYC / RESPND-MI study — a rapid, community-led response to the 2022 mpox outbreak among queer and trans New Yorkers. This repository contains the Quarto book, data-processing pipeline, and supporting R functions used to generate the public report at [https://mpxresponse.org](https://mpxresponse.org).

## Overview

This repository integrates documentation, data processing, and analytical code in one reproducible research environment. It was built with the following goals:

-   Combine **narrative and analysis** in a single Quarto book project
-   Support **reproducible pipelines** through the `targets` package
-   Enable **open collaboration** among community, academic, and technical partners

The repository doubles as both a **Quarto publication** and a **computational analysis project**. The rendered book is published to GitHub Pages (see `CNAME`) and mirrored at [mpxresponse.org](https://mpxresponse.org).

## Directory Structure

### Core configuration files

| Path                                        | Description                                                       |
|----------------------------------------------|------------------------------------|
| `_quarto.yml`                                | Master Quarto configuration: book structure, theme, chapter order |
| `_targets.yaml`                              | Points the `targets` package at `targets/_targets.R`              |
| `_config.json`                               | Settings for computation and rendering, including data filepaths  |
| `_commuity_connection_report.Rproj`          | RStudio project file                                               |
| `CNAME`                                      | Custom domain configuration for GitHub Pages                      |
| `.gitignore`                                 | Excludes RStudio/R artifacts and the `targets` cache               |

### Report chapters (root-level `.qmd` files)

Each numbered file is a chapter of the main report, in the order defined in `_quarto.yml`. Most chapters assemble their content from `{{< include >}}` calls into a matching `__<chapter>/` folder (see below).

| File                 | Section                        |
|----------------------|---------------------------------|
| `index.qmd`          | Home page                      |
| `1_contents.qmd`     | Contents (abstract, at-a-glance) |
| `2_intro.qmd`        | Introduction                   |
| `3_data_methods.qmd` | Methods — Data                 |
| `4_measures.qmd`     | Methods — Measures              |
| `5_analysis.qmd`     | Methods — Analysis              |
| `6_people.qmd`       | Results — People                |
| `7_gatherings.qmd`   | Results — Gatherings             |
| `8_movement.qmd`     | Results — Movement               |
| `9_mixing.qmd`       | Results — Mixing                 |
| `10_outbreaks.qmd`   | Results — Outbreaks               |
| `11_discussion.qmd`  | Discussion / Conclusion         |
| `0_methods.qmd`      | Part-title stub for the Methods section |
| `404.qmd`            | Custom 404 page                |

### Appendices (lettered `.qmd` files)

| File               | Section                                             |
|--------------------|------------------------------------------------------|
| `A_context.qmd`    | SSNAC I: Context                                    |
| `B_description.qmd`| SSNAC II: Description                               |
| `C_causality.qmd`  | SSNAC III: Causality                                |
| `D_organizing.qmd` | RESPND-MI: Organizing                               |
| `E_marketing.qmd`  | RESPND-MI: Marketing                                |
| `F_measurement.qmd`| MPX NYC: Measurement (incl. the person/place/network mapper) |
| `G_results.qmd`    | MPX NYC: Supplementary Results (people, places, movement & mixing) |

### Included content folders

Each chapter/appendix pulls its prose from a matching double-underscore-prefixed folder of the same name (e.g. `2_intro.qmd` includes files from `__0_index/`, `6_people.qmd` from `__6_people/`, `A_context.qmd` from `__A_context/`, etc.):

| Folder             | Feeds into        |
|--------------------|-------------------|
| `__0_index/`       | `index.qmd`       |
| `__1_contents/`    | `1_contents.qmd`  |
| `__6_people/`      | `6_people.qmd`    |
| `__8_movement/`    | `8_movement.qmd`  |
| `__A_context/`     | `A_context.qmd`   |
| `__B_description/` | `B_description.qmd` |
| `__C_causality/`   | `C_causality.qmd` |
| `__E_marketing/`   | `E_marketing.qmd` |
| `__F_measurement/` | `F_measurement.qmd` (includes the standalone `person_place_network_mapper.qmd` visualization) |
| `__G_results/`     | `G_results.qmd`   |

Reusable content shared across chapters lives in triple-underscore-prefixed folders:

| Folder             | Purpose                                              |
|--------------------|-------------------------------------------------------|
| `___definitions/`  | Formal definitions used throughout the causality/appendix chapters (one `.qmd` per term) |
| `___equations/`    | Numbered equations referenced across chapters          |
| `___figures/`      | Figure blocks (one `.qmd` per figure) referenced via cross-references |
| `___tables/`       | Table blocks (one `.qmd` per table)                    |
| `___videos/`       | Embedded video blocks, one per chapter/topic           |
| `___misc/`         | Miscellaneous shared snippets (e.g. seminar banner)    |
| `____data/`        | R scripts defining/loading the person- and place-level survey data objects |

Build output and knitr caches (not hand-edited) also live alongside the source: `_book/` (rendered HTML site), `index_files/`, `C_causality_files/`, `C_causality_cache/`, and per-appendix `_files`/`_cache` folders generated on render.

### Analytical pipeline

| Folder          | Purpose                                                                 |
|-----------------|--------------------------------------------------------------------------|
| `R_functions/`  | Custom R functions, organized by pipeline stage (`0_calculated_variables/` … `11_table_function_appendices/`) covering data cleaning, analysis, and figure/table generation for both the main report and appendices |
| `targets/`      | The `{targets}` pipeline definition (`_targets.R`) and its build cache (`_targets/`, gitignored) |

### Supporting assets

| Folder          | Purpose                                                                 |
|-----------------|--------------------------------------------------------------------------|
| `_const/`       | Shared constants and assets: SCSS themes (`custom_theme.scss`, `custom_index.scss`), bibliography (`bibliography.bib`), the participant questionnaire (`questionnaire.json`), reference images, emoji icons, source documents, and a small JS helper (`scroll.js`) |
| `_extensions/`  | Quarto extensions (currently the `fontawesome` shortcode extension)     |
| `images/`       | Miscellaneous images pasted directly into `.qmd` files                 |

## Running the Analysis

### 1. Install Dependencies

``` r
install.packages(c(
  "cowplot",
  "dplyr",
  "ggforce",
  "ggimage",
  "ggplot2",
  "ggraph",
  "grid",
  "gt",
  "gtsummary",
  "here",
  "igraph",
  "knitr",
  "labelled",
  "latex2exp",
  "lubridate",
  "magick",
  "purrr",
  "remotes",
  "rlang",
  "scales",
  "sf",
  "stringr",
  "targets",
  "tidygraph",
  "tidyjson",
  "tidyr",
  "uuid"
))

remotes::install_github("mpxnyc/mpxnyc")
```

### 2. Rebuild the Data Pipeline

``` r
targets::tar_source("R_functions") # Load custom functions
targets::tar_make()                # Run the pipeline defined in targets/_targets.R
```

This regenerates all intermediate objects, figures, and derived data required by the Quarto report.

### 3. Render the Report

From the project root:

``` bash
quarto render .
```

The compiled HTML files appear under `_book/`.

## Reproducibility

All analytic steps are defined in `_targets.yaml` / `targets/_targets.R` and the functions in `R_functions/`.
Each Quarto chapter can be compiled independently or as part of the complete book.
Dependencies are managed through `targets`.

## Collaboration

To contribute:

1.  Fork the repository and create a new branch.
2.  Add or modify Quarto sections, R functions, or documentation.
3.  Ensure the Quarto book compiles without errors (`quarto render`).
4.  Submit a pull request describing the changes.

All contributions should follow the principles of **community accountability**, **transparency**, and **reproducibility**.

## Citation

Makofane K, et al. *MPX NYC: A Community-Led Study of Networks, Outbreaks, and Connection.*
RESPND-MI, 2025. <https://mpxresponse.org>

## Maintainers

**Principal Investigator:** [Keletso Makofane, MPH, PhD](https://keletsomakofane.com)
**Contact:** admin@controlf.info

## Acknowledgments

We thank the participants and collaborators whose work and trust made this project possible.
*"Community is a form of infrastructure. This project shows what happens when we treat it that way."*
