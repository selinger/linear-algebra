TEXNAME=LinearAlgebra

export TEXINPUTS=.:../styles:

ebook: $(TEXNAME).pdf

print: print-cover-crown-quarto.pdf interior-crown-quarto.pdf interior-bw-crown-quarto.pdf

all: ebook print

index: $(TEXNAME).ind

print-cover: print-cover.pdf
print-cover.dvi: $(TEXNAME).tex frontmatter/titlepage.tex frontmatter/blurb.tex figures/titlepage-image-print1.eps figures/titlepage-image-print2.eps
	latex --jobname print-cover "$<"
	latex --jobname print-cover "$<"

print-cover-crown-quarto: print-cover-crown-quarto.pdf
print-cover-crown-quarto.dvi: $(TEXNAME).tex frontmatter/titlepage.tex frontmatter/blurb.tex figures/titlepage-image-print1.eps figures/titlepage-image-print2.eps
	latex --jobname print-cover-crown-quarto "$<"
	latex --jobname print-cover-crown-quarto "$<"

print-cover-front: print-cover-front.pdf
print-cover-front.dvi: $(TEXNAME).tex frontmatter/titlepage.tex figures/titlepage-image.eps
	latex --jobname print-cover-front "$<"
	latex --jobname print-cover-front "$<"

interior: interior.pdf
interior.dvi: $(TEXNAME).tex revid.tex
	latex --jobname interior "$<"
	makeindex interior "$<"
	latex --jobname interior "$<"
	latex --jobname interior "$<"

interior-bw: interior-bw.pdf
interior-bw.dvi: $(TEXNAME).tex revid.tex
	latex --jobname interior-bw "$<"
	makeindex interior-bw "$<"
	latex --jobname interior-bw "$<"
	latex --jobname interior-bw "$<"

interior-crown-quarto: interior-crown-quarto.pdf
interior-crown-quarto.pdf: interior.pdf
	gs -o "$@" -SDEVICE=pdfwrite -dNOPAUSE -dBATCH -dSAFER -dQUIET -dDEVICEWIDTHPOINTS=536 -dDEVICEHEIGHTPOINTS=697 -dFIXEDMEDIA -dCompatibilityLevel=1.4 -dPDFFitPage -dAutoRotatePages=/None "$<"

interior-bw-crown-quarto: interior-bw-crown-quarto.pdf
interior-bw-crown-quarto.pdf: interior-bw.pdf
	gs -o "$@" -SDEVICE=pdfwrite -dNOPAUSE -dBATCH -dSAFER -dQUIET -dDEVICEWIDTHPOINTS=536 -dDEVICEHEIGHTPOINTS=697 -dFIXEDMEDIA -dCompatibilityLevel=1.4 -dPDFFitPage -dAutoRotatePages=/None "$<"

$(TEXNAME).dvi: $(TEXNAME).tex revid.tex
	latex "$<"
	makeindex "$(TEXNAME)"
	latex "$<"
	latex "$<"

%.ps: %.dvi
	dvips -o "$*".ps "$<" 

%.pdf: %.ps
	ps2pdf -r300 -dPDFSETTINGS=/printer -dColorConversionStrategy=/LeaveColorUnchanged -dGrayImageResolution=300 -dColorImageResolution=300 -dAutoRotatePages=/None "$<"

# Intermediate and target files that Latex will output
suffixes := .dvi .ps .pdf .out .log .toc .aux .idx .ilg .ind 

.PHONY: clean
clean:
	rm -f $(foreach f,$(suffixes),$(TEXNAME)$(f))
	rm -f *-answers.tex
	rm -f interior.* interior-bw.* interior-crown-quarto.* interior-bw-crown-quarto.*
	rm -f print-cover.* print-cover-crown-quarto.* print-cover-front.*

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
