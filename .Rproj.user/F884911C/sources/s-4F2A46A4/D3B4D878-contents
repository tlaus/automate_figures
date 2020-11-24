## Make Supplementary Figures - universal
## Tereza Lausová
## 23.11.2020

## libraries
if (!require("packrat")) install.packages("packrat", lib = Sys.getenv("R_LIBS_USER"))
packrat::on()
library(here) # sets the root directory as the directory from which the R script is run, or Rproject started
library(purrr) # fast for


## SNPs
directory <- paste0(here("figures"), "/") # path relative to root directory
files <- list.files(here("figures"))

figures_tex <- map_chr(files, .f = function(figure) {
  paste0(
    '\\subfloat[]{\\includegraphics[width = 0.3\\textwidth]{"',  ## Backslash needs to be escaped with another backslash
    directory,
    figure,
    '"}}'
  )
})

preamble <- c(
  ## This is only needed for testing, produces errors when trying to \include in latex
#   '\\documentclass[paper=a4,parskip=half,headings=normal]{scrreprt}
# \\usepackage[utf8]{inputenc} 
# \\usepackage[T1]{fontenc} 
# \\usepackage[english]{babel} 
# 
# \\usepackage{graphicx}
# \\usepackage{float}
# \\usepackage[caption = false]{subfig}
# 
# %%--------------------------------------
# 
# \\begin{document}
'\\begin{figure}
'
)

linebreak <- '\\\\' # two backslashes, two escaping backslashes

pagebreak <- function(command){paste0(
  '\\caption{Caption for the figures.}
  \\label{supp-fig-', command/12,'}
  \\end{figure}
  
  \\begin{figure}'
)}

pagebreak_nobegin <- function(command){paste0(
  '\\caption{Caption for the figures.}
  \\label{supp-fig-', command/12,'}
  \\end{figure}'
)}

## add linebreaks
for (command in 1:length(figures_tex)) {
  if (command%%3 == 0) {
    figures_tex[command] <- paste0(figures_tex[command], linebreak)
  }
}

## add pagebreaks
for (command in 1:length(figures_tex)) {
  if ((length(figures_tex) == command) && (length(figures_tex) %% 12 == 0)) {
    figures_tex[command] <- paste0(figures_tex[command],"\n", pagebreak_nobegin(command), "\n")
    
    break
  }
  if (command%%12 == 0) {
    figures_tex[command] <- paste0(figures_tex[command],"\n", pagebreak(command), "\n")
  }
}

docend <- paste0(
  '\\caption{Caption for the figures.}
  \\label{supp-fig-', ceiling(length(figures_tex)/12),'}
  \\end{figure}
  \\end{document}'
)

if (length(figures_tex) %% 12 == 0) {
  docend <- '\\end{document}'
}else{
  docend <- paste0(
    '\\caption{Caption for the figures.}
  \\label{supp-fig-', ceiling(length(figures_tex)/12),'}
  \\end{figure}
  \\end{document}'
  )
}

writeLines(c(preamble, figures_tex, docend), con = here("LaTeX/supplementary_figures.tex"), sep = "\n", useBytes = FALSE)