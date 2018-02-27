
SHELL := /bin/bash

TEX := teaching-preferences.tex
PDF := $(TEX:%.tex=%.pdf)

PDFLATEX := /opt/texlive/2017/bin/x86_64-linux/pdflatex

all: $(PDF)

%.pdf: %.tex
	$(PDFLATEX) $<
	$(PDFLATEX) $<
	$(PDFLATEX) $<

clean:
	rm -f $(basename $(TEX)).{aux,fls,log,out,pdf}

