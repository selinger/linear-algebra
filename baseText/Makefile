#use Texlive 2015
export PATH := /usr/local/texlive/2015/bin/x86_64-linux:$(PATH)

TEXNAME=LinearAlgebra

export TEXINPUTS=.:../styles:

all: $(TEXNAME).pdf

index: $(TEXNAME).ind

print-cover: print-cover.pdf
print-cover.dvi: $(TEXNAME).tex frontmatter/titlepage.tex frontmatter/blurb.tex figures/titlepage-image-print1.eps figures/titlepage-image-print2.eps
	latex --jobname print-cover "$<"
	latex --jobname print-cover "$<"
	latex --jobname print-cover "$<"

print-cover-crown-quarto: print-cover-crown-quarto.pdf
print-cover-crown-quarto.dvi: $(TEXNAME).tex frontmatter/titlepage.tex frontmatter/blurb.tex figures/titlepage-image-print1.eps figures/titlepage-image-print2.eps
	latex --jobname print-cover-crown-quarto "$<"
	latex --jobname print-cover-crown-quarto "$<"
	latex --jobname print-cover-crown-quarto "$<"

print-cover-front: print-cover-front.pdf
print-cover-front.dvi: $(TEXNAME).tex frontmatter/titlepage.tex figures/titlepage-image.eps
	latex --jobname print-cover-front "$<"
	latex --jobname print-cover-front "$<"
	latex --jobname print-cover-front "$<"

base-text: base-text.pdf
base-text.dvi: $(TEXNAME).tex
	latex --jobname base-text "$<"
	makeindex base-text "$<"
	latex --jobname base-text "$<"
	latex --jobname base-text "$<"

interior: interior.pdf
interior.dvi: $(TEXNAME).tex revid.tex
	latex --jobname interior "$<"
	makeindex interior "$<"
	latex --jobname interior "$<"
	latex --jobname interior "$<"

interior-crown-quarto: interior-crown-quarto.pdf
interior-crown-quarto.pdf: interior.pdf
	gs -o "$@" -SDEVICE=pdfwrite -dDEVICEWIDTHPOINTS=536 -dDEVICEHEIGHTPOINTS=697 -dFIXEDMEDIA -dCompatibilityLevel=1.4 -dPDFFitPage -dAutoRotatePages=/None "$<"

%.dvi : %.tex revid.tex
	latex "$<"
	makeindex $(TEXNAME)
	latex "$<"
	latex "$<"

%.ps : %.dvi
	dvips -o "$*".ps "$<" 

%.pdf : %.ps
	ps2pdf -r300 -dPDFSETTINGS=/printer -dColorConversionStrategy=/LeaveColorUnchanged -dGrayImageResolution=300 -dColorImageResolution=300 -dAutoRotatePages=/None "$<"

.SECONDARY: interior.ps

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

spellcheck:
	ispell -d canadian -p ./dictionary.txt -t frontmatter/*.tex content/*.tex exercises/*.tex

.PHONY: revid.tex
revid.tex:
	REV=`git rev-parse HEAD | sed 's/\(.......\).*/\1/'`;\
	DATE=`git log HEAD^- | grep '^Date:' | sed 's/Date://;s/\([0-9]*:[0-9]*:[0-9]*\) \+\([0-9][0-9][0-9][0-9]\)/\2 \1/'`;\
	FDATE=`date "+%B %e, %Y" -d "$$DATE" --utc`;\
	echo "\\\newcommand{\\\revid}{$$REV}" > "revid-new.tex";\
	echo "\\\newcommand{\\\revdate}{$$FDATE}" >> "revid-new.tex";\
	if test ! -e "$@"; then touch "$@"; fi;\
	if diff -q "revid-new.tex" "$@"; then rm -f "revid-new.tex"; else mv -f "revid-new.tex" "$@"; fi;\
	echo "Revision $$REV of $$FDATE"
