# Versioned Report

Versioned report is a template to easily and efficiently keep track of changes in a project, especially in the context of clinical research.

## Setup

First thing first, you need to assure you have the following snippets in your RStudio environment. This will allow you to quickly insert the necessary tables and navigation elements at the beginning of your Quarto reports.

To have them available, copy the following codes at the end of the Markdown section of your RStudio snippets file, which you can find at:
Tools -> Edit code snippet ... -> Markdown

```markdown
snippet vlog
	# 📝 DIARIO DELLE MODIFICHE
	| File/Ver. | Data | Richiedente | Oggetto della Richiesta | Modifiche Apportate |
	|:---|:---|:---|:---|:---|
	| `${1:v01}` | `r Sys.Date()` | ${2:Nome Clinico/PI} | ${3:Analisi Iniziale} | ${4:Setup progetto} |

snippet vrow
	| `${1:v0X}` | `r Sys.Date()` | ${2:Nome} | ${3:Nuova richiesta} | ${4:Dettagli modifiche} |

snippet nav
	::: {.callout-note appearance="simple" icon=false}
	## 🧭 Navigazione Versioni
	| **⏮️ Precedente** | **📍 Questo File** | **⏭️ Successivo** |
	|:---:|:---:|:---:|
	| `${1:nessuno}` | **`r knitr::current_input()`** | `${2:(da assegnare)}` |
	:::
```

## Usage

### Versioning of reports

To use the snippets, simply type `nav` to insert the navigation table at the top of your report, `vlog` to insert the initial version log table just below the navigation one, and, next, `vrow` to add a new row for each subsequent change, i.e. every new report file you produce; just before to create it.

For example, suppose you are on version V2 of the analysis, when you decide to make a new version, IN the v02 file, you modify the "**⏭️ Next**" field by writing `report_v03.qmd`, save, and THEN do "Save As -> v03." In V03, add a row to the table (with `vrow`), and in the "**⏮️ Previous**" field, write `report_v02.qmd`, save, and from there, update and continue with your analysis.

This way, the v02 report will always say: "The next one is v03" (and it will have said "The previous one was v01"!); and in report v03 it will say "The previous one was v02" and when you update it, it will say and it will be known that "the next one is v04_ab" (the point is not to impose a rigid naming convention, but to keep track of the chain of updates).

### Issues and diff between versions

The tracking and reportign of the changes between version has some prerequisites form the procedural point of view, in particolar in the the usage of git and GitHub. In particular, you need to have a repository with the project files, and to use issues to keep track of the changes you want to make, put the tags `#<issue_id>`  in the commit message for each issue involved in the code commited, and to use tags to mark the different versions of your project. For example, you can use tags like `v1`, `v2`, etc., or you can use more descriptive tags like `analysis_v1`, `analysis_v2`, etc.

::: {.callout-note}
commit tags and versioned file names does not need to be the same. Obviously, if you use the same tag for both, it will be easier to keep track of the changes, but the two systems are independent, and you can use different tags for the commits and for the file names. The important thing is to have a clear and consistent system for tracking the changes, and to use it consistently throughout the project.
:::

To keep track of the changes between versions, you can than use the `changelog.qmd` script template. Tu execute it, simply run the following command in your R console (changing the tag names with the ones of your interest that you are using in your project):

```r
quarto::quarto_render(
  input = "changelog.qmd",
  output_file = "Report_Modifiche_v1_v2.docx",
  execute_params = list(
    tag_start = "v1",  # Il tag (o hash commit) di partenza
    tag_end = "v2"     # Il tag (o hash commit) attuale
  )
)
```
 
