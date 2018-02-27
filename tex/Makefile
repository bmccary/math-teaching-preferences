
SHELL := /bin/bash

TEX := teaching-preferences.tex
PDF := $(TEX:%.tex=%.pdf)

# Old versions of TeXLive have a partially-broken implementation
# of PDF-generating utilities related to the /AD tag, but this
# is fixed on TeXLive 2017+. There is such an installation on
# regulus.
#
# PDFLATEX := /opt/texlive/2017/bin/x86_64-linux/pdflatex

PDFLATEX := pdflatex

all: $(PDF)

%.pdf: %.tex
	$(PDFLATEX) $<
	$(PDFLATEX) $<
	$(PDFLATEX) $<

clean:
	rm -f $(basename $(TEX)).{aux,fls,log,out,pdf}

