MATRIX THEORY AND LINEAR ALGEBRA
================================

This work is covered by a Creative Commons BY 4.0 License. Please see
the LICENSE file for the full text.


Code Organization
-----------------

Extract the .zip file on your computer.  It will contain two directories,
"baseText" and "styles".  Both of these directories need to be side-by-side
to compile the textbook.  The main textbook content is in "baseText".

The top-level Latex file is "LinearAlgebra.tex".  This file lists all
of the required packages and imports the file "courseSetup.tex" that
defines the content to be included in the textbook.


Compiling
---------

This textbook is designed to be compiled using TeX Live 2017 but newer
versions may work as well.  TeX Live can be downloaded from
https://www.tug.org/texlive/ .

The Makefile:

This is provided for convenience to compile the textbook.  It invokes the
typical "latex, dvips, ps2pdf" chain of commands.  We use the "--jobname"
argument to latex to compile variations on the textbook such as for
print-on-demand services.

Run "make" to compile the main textbook, including cover page and solutions.

Run "make print-cover" to compile the cover page only.

Run "make interior" to compile the interior of a print book without the cover page.

Run "make clean" to clean directory and remove unnecessary files after compiling. 
