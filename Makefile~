#
# Makefile (gnu make)
#
# A GNU makefile for creating DVI, PS, and PDF documents from LaTeX.
#
# Allows the user to specify for input:
# - a list of .tex files
# - a .bib file
# - a list of .eps files
#
# ...and for output:
# - a method for creating PDFs
#
# Lawrence You <you@cs.ucsc.edu>
#

##############################################################################
# configuration parameters (BEGIN)
##############################################################################

PAPER = uctest
DIRNAME = thesis-phd
GRAPHICS =
BIBS = uctest.bib
BIBTEX = bibtex
TEX = latex
EPSGRAPHICSFILES =

# PDFMETHOD is one of {pdftex, dvipdfm, ps2pdf}
# for PDF output, pdftex is probably best
PDFMETHOD = pdftex

default: view

# other likely defaults that you can use
#default: view-mac
#default: $(PAPER).pdf
#default: $(PAPER).dvi

##############################################################################
# configuration parameters (END)
##############################################################################



##############################################################################
# .pdf, .ps, .dvi - Acrobat, PostScript, and DVI files (BEGIN)
##############################################################################

# Three methods for generating PDF from TeX documents (choose one)
# 1. pdftex   (in teTeX) seems to have problems with embedded .eps directly
#             so we convert EPS to PDF first by using epstopdf
#             see http://www.2pi.info/latex/Includingeps.html
# 2. dvipdfm  works great, requires installation of dvipdfm
# 3. ps2pdf   (in teTeX) make sure dvips uses "-Ppdf" (may have encoding
#             problems with ligatures on older versions)

# PDFMETHOD is one of {pdftex, dvipdfm, ps2pdf}, defined above

##############################################################################
# 1. pdftex (actually pdflatex) method
##############################################################################
ifeq ($(PDFMETHOD), pdftex)

#
# a. first convert EPS to PDF
#

%.pdf : %.eps
	epstopdf $< || (echo "must convert EPS to PDF before including them with pdftex"; exit 1)

PDFGRAPHICSFILES := $(patsubst %.eps,%.pdf,$(EPSGRAPHICSFILES))

#
# b. use pdflatex to combine the PDFs containing EPS into the LaTeX-generated PDF
#

TEX = pdflatex
$(PAPER).pdf: $(PAPER).tex *.tex $(BIBS) $(PDFGRAPHICSFILES)
	$(TEX) $(PAPER); ${BIBTEX} $(PAPER); $(TEX) $(PAPER); $(TEX) $(PAPER)
endif # pdftex


##############################################################################
# 2. dvipdfm method
##############################################################################
ifeq ($(PDFMETHOD), dvipdfm)

$(PAPER).pdf: $(PAPER).dvi
	dvipdfm $(PAPER).dvi || (echo "You might need to install dvipdfm from a package or built it from source."; exit 1)

$(PAPER).ps: $(PAPER).dvi
	dvips -t letter $(PAPER) -o

$(PAPER).dvi: $(PAPER).tex *.tex $(BIBS) $(GRAPHICS) $(EPSGRAPHICSFILES)
	$(TEX) $(PAPER); ${BIBTEX} $(PAPER); $(TEX) $(PAPER); $(TEX) $(PAPER)

endif # dvipdfm


##############################################################################
# 3. ps2pdf method
##############################################################################
ifeq ($(PDFMETHOD), ps2pdf)

$(PAPER).pdf: $(PAPER).ps
	ps2pdf $(PAPER).ps

$(PAPER).ps: $(PAPER).dvi
	dvips -Ppdf -t letter $(PAPER) -o

$(PAPER).dvi: $(PAPER).tex *.tex $(BIBS) $(GRAPHICS) $(EPSGRAPHICSFILES)
	$(TEX) $(PAPER); ${BIBTEX} $(PAPER); $(TEX) $(PAPER); $(TEX) $(PAPER)

endif # ps2pdf

##############################################################################
# .pdf, .ps, .dvi - Acrobat, PostScript, and DVI files (END)
##############################################################################




##############################################################################
# common rules
##############################################################################

dvi: $(PAPER).dvi

pdf: $(PAPER).pdf

#ps: $(PAPER).ps -- not defined because it .ps is not required by all PDFMETHODs

#
# clean: remove intermediate files (leave .ps and .pdf alone)
#
clean:
	$(RM) -f *.aux *.bbl *.blg *.dvi *.lof *.log *.lot *.toc $(PAPER).out

#
# clean-all: all files PLUS pdf and ps files
#
clean-all: clean
	$(RM) -f $(PAPER).pdf $(PAPER).ps
	$(RM) -f $(PDFGRAPHICSFILES)
#	cd data && make clean


#
# view - view the PDF document
#
# on Mac OS X, open a document using the "open" command, e.g.
#  open $(PAPER).pdf
# on Cygwin, open a document using the "cygstart" (cygstart.exe) command, e.g.
#  cygstart $(PAPER).pdf

view: pdf
	open $(PAPER).pdf

view-cygwin: pdf
	cygstart $(PAPER).pdf


#
# release - create a package for distribution
#
release: clean-all
	tar -C .. -cvf ../$(DIRNAME).tar $(DIRNAME) && gzip ../$(DIRNAME).tar
	#
	# you will find the .tar.gz file in the parent directory
	#


##############################################################################
# Custom rules
##############################################################################

