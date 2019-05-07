# Erstellung von interaktiven Tutorials mit R

<h1>Inhaltsübersicht<span class="tocSkip"></span></h1>
<div class="toc"><ul class="toc-item"><li><span><a href="#Tutorial-mit-Paket-learnr-erstellen" data-toc-modified-id="Tutorial-mit-Paket-learnr-erstellen-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Tutorial mit Paket learnr erstellen</a></span><ul class="toc-item"><li><span><a href="#Installation-des-Pakets-learnr" data-toc-modified-id="Installation-des-Pakets-learnr-1.1"><span class="toc-item-num">1.1&nbsp;&nbsp;</span>Installation des Pakets learnr</a></span></li><li><span><a href="#Template-für-interaktives-Tutorial-auswählen" data-toc-modified-id="Template-für-interaktives-Tutorial-auswählen-1.2"><span class="toc-item-num">1.2&nbsp;&nbsp;</span>Template für interaktives Tutorial auswählen</a></span></li><li><span><a href="#Interaktives-Tutorial-erstellen" data-toc-modified-id="Interaktives-Tutorial-erstellen-1.3"><span class="toc-item-num">1.3&nbsp;&nbsp;</span>Interaktives Tutorial erstellen</a></span></li></ul></li><li><span><a href="#Paket-erstellen" data-toc-modified-id="Paket-erstellen-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Paket erstellen</a></span><ul class="toc-item"><li><span><a href="#Pakete-installieren" data-toc-modified-id="Pakete-installieren-2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>Pakete installieren</a></span></li><li><span><a href="#Projekt-für-Paketerstellung-starten" data-toc-modified-id="Projekt-für-Paketerstellung-starten-2.2"><span class="toc-item-num">2.2&nbsp;&nbsp;</span>Projekt für Paketerstellung starten</a></span></li><li><span><a href="#Paket-Abhängigkeiten-definieren" data-toc-modified-id="Paket-Abhängigkeiten-definieren-2.3"><span class="toc-item-num">2.3&nbsp;&nbsp;</span>Paket-Abhängigkeiten definieren</a></span></li><li><span><a href="#Ordner-anlegen" data-toc-modified-id="Ordner-anlegen-2.4"><span class="toc-item-num">2.4&nbsp;&nbsp;</span>Ordner anlegen</a></span></li><li><span><a href="#Paket-lokal-installieren" data-toc-modified-id="Paket-lokal-installieren-2.5"><span class="toc-item-num">2.5&nbsp;&nbsp;</span>Paket lokal installieren</a></span></li><li><span><a href="#Paket-starten" data-toc-modified-id="Paket-starten-2.6"><span class="toc-item-num">2.6&nbsp;&nbsp;</span>Paket starten</a></span></li></ul></li><li><span><a href="#Beispiele-von-learnr-Paketen" data-toc-modified-id="Beispiele-von-learnr-Paketen-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>Beispiele von learnr-Paketen</a></span></li><li><span><a href="#Literatur" data-toc-modified-id="Literatur-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Literatur</a></span></li></ul></div>


---

## Tutorial mit Paket learnr erstellen

Ausführliche Hinweise zu der Installation und Erstellung von learnr-Dokumenten sind auf dieser [R-Studio learnr Webpage](https://blog.rstudio.com/2017/07/11/introducing-learnr/) zu finden.

### Installation des Pakets learnr

Paket *learnr* installieren: 

`
install.packages('learnr')
`

---


### Template für interaktives Tutorial auswählen

Innerhalb von *R-Studio* in der Menüleiste die Option Interactive Tutorial auswählen.

*-> File -> New File -> R Markdown -> Template -> Interactive Tutorial*

Name der Datei (bspw. *lab1*) angeben und Speicherort auswählen. Es wird nun ein Ordner mit der gewünschten Bezeichnung an dem gewählten Ordnerpfad erstellt.

---


### Interaktives Tutorial erstellen

Auf diesen [R-Studio GitHub-Seiten](https://rstudio.github.io/learnr/) finden sich zahlreiche Hinweise zu den learnr-Optionen.

Beispiel für die Integration eines interaktiven Histograms:

Code für die Erstelltung des User Interface:


`{r, echo=FALSE}
sliderInput("bins", "Number of bins:", min = 1, max = 50, value = 30)
plotOutput("distPlot")
`

Code für die Erzeugung des Histogramms (in Abhängigkeit von dem Input aus dem oberen Code).

`{r, context="server"}
output$distPlot <- renderPlot({
  x <- faithful[, 2]  # Old Faithful Geyser data
  bins <- seq(min(x), max(x), length.out = input$bins + 1)
  hist(x, breaks = bins, col = 'darkgray', border = 'white')
})
`

Auf diesen Seiten von [R-Studio](https://shiny.rstudio.com/tutorial/) finden sich zahlreiche Tutoials zu der Funktionsweise und Implementierung von Shiny-Elementen. 
 

---
---


## Paket erstellen

Für die Bereitstellung der Tutorials eignet sich die Erstellung eines R-Pakets, mit dessen Hilfe das Tutorial innerhalb von R oder R-Studio bzw. in einem Jupyter Notebook mit Hilfe eines kurzen Befehls als HTML-Dokument in einem Browser gestartet werden kann. 


Für ausführliche Hinweise zu der Erstellung von R-Pakten siehe [Wickham, 2015](http://r-pkgs.had.co.nz). Eine Zusammenfassung der wichtigsten Schritte finden sich in dem GitHub-Beitrag von [Johnston](https://github.com/UofTCoders/studyGroup/blob/gh-pages/lessons/r/packages/lesson.md), an welchem sich auch die hier dargestellten Schritte orientieren. Dabei handelt es sich um ein Minimalbeispiel, welches nicht die Anforderungen für eine Veröffentlichung bei [CRAN](https://cran.r-project.org/index.html) erfüllt. Stattdessen kann das Paket bei GitHub veröffentlicht und von dort aus von Dritten installiert werden.   

Hinweis: dieses Tutorial behandelt nur die lokale Erstellung und Installation von Paketen (GitHub wird nicht thematisiert). 

### Pakete installieren

Zunächst müssen einige Pakete installiert werden:  

`install.packages(c('devtools', 'roxygen2', 'testthat', 'knitr'))`

### Projekt für Paketerstellung starten 

In R-Studio folgende Option auswählen:

 *-> New Project -> New Directory -> R Package*

Paketname eingeben (bspw. *meinpaket*) und den Ordnerpfad auswählen, in welchem das Paket installiert werden soll. Bei den Optionen **create git repository** und **open in new session** auswählen.

Dadurch wird ein neuer Ordner (der Name entspricht der Bezeichnung des Pakets - hier: *meinpaket*) in dem ausgewählten Pfad installiert.

### Paket-Abhängigkeiten definieren

Damit die interaktiven Elemente korrekt ausgeführt werden können, können wir noch definieren, welche anderen Pakete gemeinsam mit unserem Paket geladen werden sollen. Die Pakete einfügen und in der R-Console in der Projektumgebung ausführen.

`devtools::use_package('learnr') 


devtools::use_package('shiny')`

### Ordner anlegen 

Nun muss ein neuer Ordner mit der Bezeichnung **inst** in dem Ordner des Pakets (hier *meinpaket*) eingefügt werden. In dem Ordner *inst* wird zuletzt noch ein Ordner mit der Bezeichnung **Tutorials** eingefügt.


In den Ordner **Tutorials** kann nun der komplette Ordner aus Schritt 1.3 (hier würde der Ordner die Bezeichnung *lab1* haben) eingefügt werden.

### Paket lokal installieren  


Nachdem alle Schritte ausgeführt wurden, kann das Paket nun auf dem lokalen Rechner installiert werden

`
devtools::install('meinpaket')
`

### Paket starten  


Nun kann das Tutorial wie folgt aus R, R-Studio oder Jupyter Notebook gestartet werden:

`
library(shiny)
learn::run_tutorial('lab1', package = 'meinpaket')
`


## Beispiele von learnr-Paketen


Auf [GitHub](https://github.com/profandyfield/adventr) können Lernpakete von dem Autor Andy Fields bezogen werden, welche begleitend zu dem Lehrbuch "An adventure in statistics: The reality enigma." veröffentlicht wurden:

`
install.packages("remotes") #if you don’t already have it installed

library(remotes)

install_github("profandyfield/adventr")
`

Hier eine Übersicht der Lernpakete:
  
 * **adventr_02**: Data basics in R and RStudio
 * **adventr_03**: Summarizing data (introducing ggplot2)
 * **adventr_04**: Fitting models (central tendency)
 * **adventr_05**: Presenting data (summarizing groups and more ggplot2)
 * **adventr_08**: Inferential statistics and robust estimation (covers Chapter 9 too)
 * **adventr_11**: Hypothesis testing
 * **adventr_14**: The general linear model
 * **adventr_15**: Comparing two means
 * **adventr_15_rm**: Comparing two means (repeated measures)
 * **adventr_16**: Comparing several means
 * **adventr_16_rm**: Comparing several means (repeated measures)
 * **adventr_17**: Factorial designs
 * **adventr_mlm**: Multilevel models (not covered in the book)
 * **adventr_growth**: Growth models (not covered in the book)
 * **adventr_log**: Logistic regression (not covered in the book)

Die Tutorials könenn wie folgt gestartet werden:

`
library(adventr)

learnr::run_tutorial("name_of_tutorial", package = "adventr")
`

D.h. für Tutorial 3:

`
library(adventr)

learnr::run_tutorial("adventr_03", package = "adventr")
`



## Literatur

Field, A. (2016). An adventure in statistics: The reality enigma. Sage.

Wickham, H. (2015). R packages: organize, test, document, and share your code. O'Reilly Media, Inc.
