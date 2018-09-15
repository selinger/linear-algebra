TEXNAME=LinearAlgebra

export TEXINPUTS=.:../styles:

ebook: $(TEXNAME)

print: print-cover-crown-quarto interior-crown-quarto interior-bw-crown-quarto

images: smallcover.jpg

all: ebook print images

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

interior: revid interior.pdf
interior.dvi: $(TEXNAME).tex revid.tex
	latex --jobname interior "$<"
	makeindex interior "$<"
	latex --jobname interior "$<"
	latex --jobname interior "$<"

interior-bw: revid interior-bw.pdf
interior-bw.dvi: $(TEXNAME).tex revid.tex
	latex --jobname interior-bw "$<"
	makeindex interior-bw "$<"
	latex --jobname interior-bw "$<"
	latex --jobname interior-bw "$<"

interior-crown-quarto: revid interior-crown-quarto.pdf
interior-crown-quarto.pdf: interior.pdf
	gs -q -SDEVICE=pdfwrite -dNOPAUSE -dBATCH -dSAFER -dDEVICEWIDTHPOINTS=536 -dDEVICEHEIGHTPOINTS=697 -dFIXEDMEDIA -dCompatibilityLevel=1.4 -dPDFFitPage -dAutoRotatePages=/None -o "$@" "$<"

interior-bw-crown-quarto: revid interior-bw-crown-quarto.pdf
interior-bw-crown-quarto.pdf: interior-bw.pdf
	gs -q -SDEVICE=pdfwrite -dNOPAUSE -dBATCH -dSAFER -dDEVICEWIDTHPOINTS=536 -dDEVICEHEIGHTPOINTS=697 -dFIXEDMEDIA -dCompatibilityLevel=1.4 -dPDFFitPage -dAutoRotatePages=/None -o "$@" "$<"

$(TEXNAME): revid $(TEXNAME).pdf
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

.PHONY: revid
revid:
	$(MAKE) -B revid.tex
revid.tex:
	REV=`git rev-parse HEAD | sed 's/\(.......\).*/\1/'`;\
	DATE=`git log HEAD^- | grep '^Date:' | sed 's/Date://;s/\([0-9]*:[0-9]*:[0-9]*\) \+\([0-9][0-9][0-9][0-9]\)/\2 \1/'`;\
	FDATE=`date "+%B %e, %Y" -d "$$DATE" --utc`;\
	echo "\\\newcommand{\\\revid}{$$REV}" > "revid-new.tex";\
	echo "\\\newcommand{\\\revdate}{$$FDATE}" >> "revid-new.tex";\
	if test ! -e "$@"; then touch "$@"; fi;\
	if diff -q "revid-new.tex" "$@"; then rm -f "revid-new.tex"; else mv -f "revid-new.tex" "$@"; fi;\
	echo "Revision $$REV of $$FDATE"

smallcover.jpg: $(TEXNAME).pdf
	gs -q -dBATCH -dNOPAUSE -sOutputFile=- -sDEVICE=ppmraw -r144x144 -dLastPage=1 "$<" | pnmscale -height=500 | pnmnlfilt -0.5 0.8 | pnmtojpeg --quality=90 > "$@"

.PHONY: spellcheck
spellcheck:
	ispell -d canadian -p ./dictionary.txt -t frontmatter/*.tex content/*.tex exercises/*.tex

.PHONY: clean
clean:
	rm -f $(foreach f,$(suffixes),$(TEXNAME)$(f))
	rm -f *-answers.tex
	rm -f interior.* interior-bw.* interior-crown-quarto.* interior-bw-crown-quarto.*
	rm -f print-cover.* print-cover-crown-quarto.* print-cover-front.*
	rm -f revid.tex
	rm -f smallcover.jpg

