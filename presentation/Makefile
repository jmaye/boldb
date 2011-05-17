# Makefile for ETH latex presentation template
# uses pdflatex
# provided features:
# - out of source build (in build/)
# - autogeneration of pdf and ps files from SVG source and R code
# - one command complete clean (make distclean)
# - optional bibtex support
# for questions and bug reports, please contact
# Stephane Magnenat <stephane.magnenat@mavt.ethz.ch>

# name of the main .tex file, without the trailing .tex
name=presentation
# directory of the template, relative to the directory of the main .tex file
templatedir=.

# temporary variables
targetpdf=$(name).pdf
source=$(name).tex
bib=$(name).bib
svgfigs=$(wildcard figures/*.svg)
rfigs=$(wildcard figures/*.r)
genfigs=$(svgfigs:figures/%.svg=generated-figures/%.pdf) $(rfigs:figures/%.r=generated-figures/%.pdf)
deps=$(wildcard *.tex) $(wildcard figures/*.pdf) $(wildcard figures/*.jpg) $(wildcard figures/*.png) $(genfigs) Makefile


#$(targetpdf): create-working-dirs $(source) $(bib) $(deps)
$(targetpdf): create-working-dirs $(source) $(deps)
	cd build && TEXINPUTS=..:../${templatedir}:${TEXINPUTS} pdflatex ../$(name)
	#cd build && BIBINPUTS=..:../${templatedir}:${BIBINPUTS} bibtex $(name)
	#cd build && TEXINPUTS=..:../${templatedir}:${TEXINPUTS} pdflatex ../$(name)
	cd build && TEXINPUTS=..:../${templatedir}:${TEXINPUTS} pdflatex ../$(name)
	mv build/$(targetpdf) .

create-working-dirs:
	mkdir -p build

generated-figures/%.pdf: figures/%.svg
	inkscape --export-area-page --export-pdf=$@ -f=$<

generated-figures/%.pdf: figures/%.r
	cd build && R BATCH --no-save < ../$< && mv $(patsubst generated-figures/%.pdf,%.pdf,$@) ../generated-figures/

all: $(targetpdf)

clean: buildclean

buildclean:
	rm -rf build */*.Rout */.*.Rout */.RData *~ figures/*~

distclean: clean
	rm -f $(targetpdf)
	rm -rf generated-figures/* 
