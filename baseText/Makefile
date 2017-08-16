#use Texlive 2015
export PATH := /usr/local/texlive/2015/bin/x86_64-linux:$(PATH)

TEXNAME=LinearAlgebra-AFirstCourse

export TEXINPUTS=.:../styles:

all: $(TEXNAME).pdf

index: $(TEXNAME).ind

print-cover: print-cover.pdf

print-cover: print-cover.pdf
print-cover.dvi: $(TEXNAME).tex
	latex --jobname print-cover $<
	latex --jobname print-cover $<
	latex --jobname print-cover $<

print-cover-front: print-cover-front.pdf
print-cover-front.dvi: $(TEXNAME).tex
	latex --jobname print-cover-front $<
	latex --jobname print-cover-front $<
	latex --jobname print-cover-front $<

base-text: base-text.pdf
base-text.dvi: $(TEXNAME).tex
	latex --jobname base-text $<
	makeindex base-text $<
	latex --jobname base-text $<
	latex --jobname base-text $<

interior: interior.pdf
interior.dvi: $(TEXNAME).tex
	latex --jobname interior $<
	makeindex interior $<
	latex --jobname interior $<
	latex --jobname interior $<

%.dvi : %.tex
	latex $<
	makeindex $(TEXNAME)
	latex $<
	latex $<

%.ps : %.dvi
	dvips -o $*.ps $< 

%.pdf : %.ps
	ps2pdf -r300 -dPDFSETTINGS=/printer -dColorConversionStrategy=/LeaveColorUnchanged -dGrayImageResolution=300 -dColorImageResolution=300 $<


# Intermediate and target files that Latex will output
suffixes := .dvi .ps .pdf .out .log .toc .aux .bbl .blg .tps .glo .ist .acn .idx .ilg .ind

rmfiles := $(foreach f,$(suffixes),$(TEXNAME)$(f))

.PHONY: clean
clean:
	rm -f $(foreach f,$(suffixes),$(TEXNAME)$(f)) solutions/*.tex interior.* ex.tex
	rm -f $(foreach f,$(suffixes),$(EXNAME)$(f))
	rm -f $(foreach f,$(suffixes),print-cover$(f))
	rm -f $(foreach f,$(suffixes),print-cover-front$(f))
	rm -f $(foreach f,$(suffixes),base-text$(f))
#Test
