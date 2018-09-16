TEXNAME=LinearAlgebra

export TEXINPUTS=.:../styles:

ebook: spellcheck $(TEXNAME)
print-quarto: spellcheck cover-quarto interior-quarto-col interior-quarto-bw
print-letter: spellcheck cover-letter interior-letter-col interior-letter-bw
images: cover-small.jpg
all: spellcheck ebook print-quarto images
everything: spellcheck ebook print-quarto print-letter frontcover-letter images

# ----------------------------------------------------------------------
# The complete text with front cover, as a single PDF, letter size

$(TEXNAME): revid $(TEXNAME).pdf
$(TEXNAME).dvi: $(TEXNAME).tex revid.tex
	latex "$<"
	makeindex "$(TEXNAME)"
	latex "$<"
	latex "$<"

# ----------------------------------------------------------------------
# The interior without cover, various sizes (letter or quarto) and
# prints (color or black-and-white).

interior-letter-col: revid interior-letter-col.pdf
interior-letter-col.dvi: $(TEXNAME).tex revid.tex
	latex --jobname interior-letter-col "$<"
	makeindex interior-letter-col "$<"
	latex --jobname interior-letter-col "$<"
	latex --jobname interior-letter-col "$<"

interior-letter-bw: revid interior-letter-bw.pdf
interior-letter-bw.dvi: $(TEXNAME).tex revid.tex
	latex --jobname interior-letter-bw "$<"
	makeindex interior-letter-bw "$<"
	latex --jobname interior-letter-bw "$<"
	latex --jobname interior-letter-bw "$<"

interior-quarto-col: revid interior-quarto-col.pdf
interior-quarto-col.pdf: interior-letter-col.pdf
	gs -q -SDEVICE=pdfwrite -dNOPAUSE -dBATCH -dSAFER -dDEVICEWIDTHPOINTS=536 -dDEVICEHEIGHTPOINTS=697 -dFIXEDMEDIA -dCompatibilityLevel=1.4 -dPDFFitPage -dAutoRotatePages=/None -o "$@" "$<"

interior-quarto-bw: revid interior-quarto-bw.pdf
interior-quarto-bw.pdf: interior-letter-bw.pdf
	gs -q -SDEVICE=pdfwrite -dNOPAUSE -dBATCH -dSAFER -dDEVICEWIDTHPOINTS=536 -dDEVICEHEIGHTPOINTS=697 -dFIXEDMEDIA -dCompatibilityLevel=1.4 -dPDFFitPage -dAutoRotatePages=/None -o "$@" "$<"

# ----------------------------------------------------------------------
# The complete cover (front, back, and spine with bleed) for printing,
# various sizes (letter or quarto).

cover-letter: cover-letter.pdf
cover-letter.dvi: $(TEXNAME).tex frontmatter/titlepage.tex frontmatter/blurb.tex figures/titlepage-image-print1.eps figures/titlepage-image-print2.eps
	latex --jobname cover-letter "$<"
	latex --jobname cover-letter "$<"

cover-quarto: cover-quarto.pdf
cover-quarto.dvi: $(TEXNAME).tex frontmatter/titlepage.tex frontmatter/blurb.tex figures/titlepage-image-print1.eps figures/titlepage-image-print2.eps
	latex --jobname cover-quarto "$<"
	latex --jobname cover-quarto "$<"

# ----------------------------------------------------------------------
# The front cover only, no bleed, letter size

frontcover-letter: frontcover-letter.pdf
frontcover-letter.dvi: $(TEXNAME).tex frontmatter/titlepage.tex figures/titlepage-image.eps
	latex --jobname frontcover-letter "$<"
	latex --jobname frontcover-letter "$<"

# ----------------------------------------------------------------------
# A JPEG image of the cover, for the website

cover-small.jpg: $(TEXNAME).pdf
	gs -q -dBATCH -dNOPAUSE -sOutputFile=- -sDEVICE=ppmraw -r144x144 -dLastPage=1 "$<" | pnmscale -height=500 | pnmnlfilt -0.5 0.8 | pnmtojpeg --quality=90 > "$@"

# ----------------------------------------------------------------------
# General rules

%.ps: %.dvi
	dvips -o "$*".ps "$<" 

%.pdf: %.ps
	ps2pdf -r300 -dPDFSETTINGS=/printer -dColorConversionStrategy=/LeaveColorUnchanged -dGrayImageResolution=300 -dColorImageResolution=300 -dAutoRotatePages=/None "$<"

# ----------------------------------------------------------------------
# Compute the revision id and date from GIT. "revid" is a phony target
# which always checks the revision id, but updates revid.tex only
# if it has actually changed. When making another target, such as
# interior-quarto-col, it therefore always checks the revision,
# but rebuilds the target only if necessary.

.PHONY: revid
revid:
	$(MAKE) -B revid.tex
revid.tex:
	REV=`git rev-parse HEAD | sed 's/\(.......\).*/\1/'`;\
	DATE=`git log HEAD^- | grep '^Date:' | sed 's/Date://;s/\([0-9]*:[0-9]*:[0-9]*\) \+\([0-9][0-9][0-9][0-9]\)/\2 \1/'`;\
	FDATE=`date "+%B %e, %Y" -d "$$DATE"` ;\
	echo "\\\newcommand{\\\revid}{$$REV}" > "revid-new.tex";\
	echo "\\\newcommand{\\\revdate}{$$FDATE}" >> "revid-new.tex";\
	if test ! -e "$@"; then touch "$@"; fi;\
	if diff -q "revid-new.tex" "$@"; then rm -f "revid-new.tex"; else mv -f "revid-new.tex" "$@"; fi;\
	echo "Revision $$REV of $$FDATE"

# ----------------------------------------------------------------------
# Spell-checking

.PHONY: spellcheck
spellcheck:
	ispell -d canadian -p ./dictionary.txt -t frontmatter/*.tex content/*.tex exercises/*.tex

# ----------------------------------------------------------------------
# Cleaning

# Intermediate and target files that Latex will output
suffixes := .dvi .ps .pdf .out .log .toc .aux .idx .ilg .ind 

.PHONY: clean
clean:
	rm -f $(foreach f,$(suffixes),$(TEXNAME)$(f))
	rm -f *-answers.tex
	rm -f interior-letter-col.* interior-letter-bw.* interior-quarto-col.* interior-quarto-bw.*
	rm -f cover-letter.* cover-quarto.*
	rm -f frontcover-letter.*
	rm -f cover-small.jpg
	rm -f revid.tex

