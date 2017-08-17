A FIRST COURSE IN LINEAR ALGEBRA
===============================

This work is covered by a Creative Commons BY 4.0 License. Please see
the LICENSE file for the full text.


The Latest Version
------------------

The latest version of this textbook is distributed by Lyryx Learning Inc.
Our contact information is at the end of this file.

This textbook can be paired with our online assessment system.  We also
provide editorial assistance to customize this book for your courses.  Please
contact us for more information.


Code Organization
-----------------

Extract the .zip file on your computer.  It will contain two directories,
"baseText" and "styles".  Both of these directories need to be side-by-side
to compile the textbook.  The main textbook content is in "baseText".

The top-level Latex file is "LinearAlgebra-AFirstCourse.tex".  This file lists all
of the required packages and imports the files "courseInfo.txt" and "courseSetup.tex" that define the content to be included in the textbook.


Customizing the Textbook
------------------------

To customize the text for your course:

1: Edit courseInfo.txt

  This file contains specific course and institution information to be
  displayed on title page.

2: Edit courseSetup.tex

  This file imports the chapter and section files that will define the content
  of the textbook.  Chapters and sections can be reorded in this file or
  removed.


Compiling
---------

This textbook is designed to be compiled using TeX Live 2015 but other
versions may work as well.  TeX Live can be downloaded from
https://www.tug.org/texlive/ .  You will need to look through their archive
to find older versions of the compiler.


On Linux: Add TeX Live 2015 to the PATH in your environment or edit the PATH
  defined in the Makefile.  Then run one of the commands below.

On Windows: Install the Cygwin command-line environment from
  https://www.cygwin.com/ and add TeX Live 2015 to the PATH in Cygwin.  Then
  run one of the commands below.  Alternatively, you can use one of the many
  Latex editors or the built-in Windows command-line and simply run TeX Live
  directly.

The Makefile:

This is provided for convenience to compile the textbook.  It invokes the
typical "latex, dvips, ps2pdf" chain of commands.  We use the "--jobname"
argument to latex to compile variations on the textbook such as for
print-on-demand services.

Run "make" to compile the main textbook, including cover page and solutions.

Run "make base-text" to compile the main textbook, including cover page and solutions, containing all material with no customizations.

Run "make print-cover" to compile the cover page only.

Run "make interior" to compile the interior of a print book without the cover page.

Run "make clean" to clean directory and remove unnecessary files after compiling. 

Credits
-------

This book was forked from A First Course in Linear Algebra by Ken Kuttler.  The original text can be downloaded from 
https://www.saylor.org/site/wp-content/uploads/2012/02/Elementary-Linear-Algebra-1-30-11-Kuttler-OTC.pdf 
or 
https://math.byu.edu/~klkuttle/
but our version may have diverged substanially
from the original.


Contributing
------------

We welcome contributions from the community.  If you have new or revised
content that you wish to share, please contact us at the address below.


Contact
-------

Lyryx Learning Inc
301 14 St NW
Calgary, Alberta
T2N 2A1

Visit our web site: http://lyryx.com

General information and inquires: info@lyryx.com

