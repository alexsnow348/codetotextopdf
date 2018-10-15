# Code2Tex

Code2Tex was created to aid in grading programming assignments.  It takes
source code and inserts it into a simple LaTeX document from which a PDF can be
made.  This provides a document with syntax highlighting and clear headings for
each file that can be marked up and returned to the students.

Syntax highlighting is provided via the
[listings](https://www.ctan.org/pkg/listings) LaTeX package.  Not all languages
are currently supported; the list of included languages is in the [package
documentation](http://www.texdoc.net/texmf-dist/doc/latex/listings/listings.pdf#page=13)
(Javascript is a notable exception).  For any language that does not have
syntax highlighting rules, the file will simply be included as monospaced black
text.

## Dependencies

The python scripts require Python 3 and have no other dependencies.

Producing a PDF of the LaTeX output requires LaTeX.  Code2tex's output depends
on a few packages that are not always included by default in a LaTeX install;
in Ubuntu, for example, you'll need to install the following packages (along
with their dependencies):

    texlive-fonts-recommended
    texlive-latex-extra
    texlive-latex-extra-doc
    texlive-math-extra
    texlive-pictures-doc

Install these with the following command:

    sudo apt-get install texlive-fonts-recommended texlive-latex-extra texlive-latex-extra-doc texlive-math-extra texlive-pictures-doc

On other systems, you will need to find the correct packages.  Look for
"[latex]-extra" and "math-extra" packages.


## Usage / Examples

Run `code2tex.py` followed by any number of filenames.  It will output the
LaTeX document to standard out, which may be redirected to a file:

    ./code2tex.py file [file [file [...]]] > output.tex

This will create `output.tex`, ready to pass to `pdflatex` or `xelatex`.  For
example, if a student with user id "jsmith" submitted several java files:

    ./code2tex.py jsmith*.java > jsmith.tex
    pdflatex jsmith.tex

This would create a PDF named `jsmith.pdf` containing the contents of all
`jsmith*.java` files, nicely formatted with syntax highlighting and a separate
header for each.

If you have UTF-8 characters in a filename or within an included file, use
`xelatex` in place of `pdflatex` for better unicode handling.  The output may
look better if you have the `lmodern` package installed as well.  This should
work for Latin characters, but non-Latin characters may still render
incorrectly within a file listing.

Wildcards and [brace
expansion](https://www.gnu.org/software/bash/manual/html_node/Brace-Expansion.html)
can be used to quickly grab files of multiple extensions from within a folder:

    ./code2tex.py codefolder/*.{html,css,js} > code.tex && pdflatex code.tex

Or from multiple nested folders:

    ./code2tex.py codefolder/*/*.{html,css,js} > code.tex && pdflatex code.tex
